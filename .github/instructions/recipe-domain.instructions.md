# Recipe Domain Instructions

This instruction file helps GitHub Copilot understand recipe management domain concepts and best practices.

## Domain Overview
You're working with a Recipe Manager application that helps users store, organize, and search recipes. The application deals with culinary terminology, measurement conversions, dietary restrictions, and meal planning.

## Core Concepts

### Recipe Components
- **Ingredients**: Items needed to prepare a dish (flour, eggs, olive oil, etc.)
- **Measurements**: Standard units like cups, tablespoons (tbsp), teaspoons (tsp), grams (g), milliliters (ml), ounces (oz)
- **Instructions**: Step-by-step cooking directions
- **Metadata**: Prep time, cook time, total time, servings, difficulty level
- **Categories**: Breakfast, lunch, dinner, dessert, snack, appetizer, beverage
- **Cuisine Types**: Italian, Mexican, Chinese, Indian, French, Mediterranean, etc.

### Dietary Restrictions & Preferences
- **Vegetarian**: No meat, fish, or poultry
- **Vegan**: No animal products (meat, dairy, eggs, honey)
- **Gluten-free**: No wheat, barley, rye
- **Dairy-free**: No milk, cheese, butter
- **Nut-free**: No peanuts, tree nuts
- **Low-carb**: Reduced carbohydrate content
- **Keto**: High-fat, low-carb diet
- **Paleo**: Whole foods, no processed items

### Common Measurement Conversions
```
Volume:
- 1 cup = 16 tbsp = 48 tsp = 240 ml
- 1 tbsp = 3 tsp = 15 ml
- 1 tsp = 5 ml

Weight:
- 1 lb = 16 oz = 454 g
- 1 oz = 28 g

Temperature:
- Celsius to Fahrenheit: (C × 9/5) + 32
- Fahrenheit to Celsius: (F - 32) × 5/9
```

## Code Generation Guidelines

### Model Design
When creating recipe-related models:
- Use descriptive field names: `prep_time_minutes`, `cook_time_minutes`, not `pt`, `ct`
- Store measurements consistently (convert to standard units)
- Include validation for sensible ranges (cooking time > 0, servings >= 1)
- Support optional fields for dietary tags, cuisine types

Example structure:
```python
class Recipe:
    id: int
    name: str
    ingredients: List[Ingredient]
    instructions: List[str]
    prep_time_minutes: int
    cook_time_minutes: int
    servings: int
    dietary_tags: List[str]  # ["vegetarian", "gluten-free"]
    cuisine: Optional[str]
```

### Search & Filtering
When implementing search features:
- Support ingredient-based search ("recipes with chicken")
- Enable dietary filtering ("show vegan recipes")
- Allow time-based queries ("quick recipes under 30 minutes")
- Consider fuzzy matching for ingredient names (tomato vs tomatoes)

### Validation Logic
When validating recipe data:
- Check for empty ingredient lists
- Verify time values are positive integers
- Ensure serving count is at least 1
- Validate dietary tags against known list
- Check measurement units are recognized

### User-Friendly Output
When formatting recipe information:
- Display prep/cook times in human-readable format ("25 minutes", not "25")
- Show ingredients with proper measurements ("2 cups flour", not "flour: 2")
- Format instructions as numbered steps
- Include dietary icons/labels prominently

## Best Practices

### Data Consistency
- Store all times in minutes (convert for display)
- Use lowercase for dietary tags to avoid duplicates
- Normalize ingredient names (remove extra spaces, standardize plurals)
- Store temperatures in Celsius (convert for display based on locale)

### Error Handling
- Provide clear error messages ("Recipe must have at least one ingredient")
- Suggest corrections for misspelled ingredients
- Warn about unusual values (10-hour prep time, 100 servings)

### Testing Scenarios
When writing tests, consider:
- Edge cases: 0 ingredients, extremely long cooking times, special characters in names
- Dietary conflicts: recipe marked "vegan" but contains eggs
- Search accuracy: partial matches, case sensitivity
- Measurement conversions: precision and rounding

## Common Pitfalls to Avoid
- Don't mix measurement systems without conversion (cups and ml in same recipe)
- Don't assume all recipes fit standard categories (fusion cuisine, experimental dishes)
- Don't validate dietary restrictions too strictly (users may have custom tags)
- Don't ignore cultural naming conventions (different terms for same ingredient)

## Terminology Guide
- **Mise en place**: Having all ingredients prepared before cooking
- **Sauté**: Cooking quickly in hot pan with small amount of fat
- **Simmer**: Cooking liquid just below boiling point
- **Blanch**: Brief boiling followed by ice water bath
- **Season to taste**: Add salt/pepper according to preference
- **Garnish**: Decorative edible addition to finished dish

---

Use this context to generate accurate, user-friendly code for the Recipe Manager application. Prioritize data integrity, clear validation messages, and helpful search functionality.

