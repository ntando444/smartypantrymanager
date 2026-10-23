# smartypantrymanager
A Java Android application for recording food already at home and finding recipes that can be made without missing ingredients. Recipe suggestions use a strict match: every required ingredient must exist in the pantry in a sufficient quantity.

## Technology

- Native Android app written in Java
- Local SQLite database through `SQLiteOpenHelper`
- RecyclerView with custom ingredient and recipe adapters
- No account, cloud service, GPS, mapping SDK, or network permission

SQLite is used because this assignment is a single-device pantry tracker. It stores the pantry, seeded recipes, recipe ingredients, and preferences on the device, works offline, and demonstrates create/read/update/delete operations without requiring a server account.

## Features in this project

- Add, edit, list, and delete pantry ingredients, with quantity, unit, and optional ISO expiry date (`YYYY-MM-DD`)
- 18 recipes seeded the first time the database is created
- Strict recipe suggestions; partial matches are excluded
- Ingredient name normalization for plurals and common alternatives (for example, tomato/tomatoes and courgette/zucchini)
- Quantity checks and conversions between grams/kilograms and millilitres/litres
- Recipe detail screen with ingredients and preparation method
- Persisted expiry-reminder and preferred-unit settings; the reminder setting highlights expired or soon-to-expire pantry rows
- Empty-state feedback and input validation

## Open and run

1. Install Android Studio and its Android SDK (API 35).
2. Open this `android-smart-pantry` folder in Android Studio.
3. Allow Gradle sync and dependency downloads to complete.
4. Create or select an Android emulator (API 23 or newer), then press **Run**.
5. Add pantry ingredients, open **Recipes**, and test the strict-match rule by changing or deleting one required item.

The app is a native Android Studio project; it is not an Expo app or a browser preview. This workspace does not include an Android emulator, so install and run it in Android Studio before treating the UI as demonstrated.

## Project map

- `app/src/main/java/.../ui/` — five Activities and custom RecyclerView adapters
- `app/src/main/java/.../data/PantryDbHelper.java` — SQLite schema and CRUD/settings operations
- `app/src/main/java/.../data/RecipeSeeder.java` — first-run seed data for 18 recipes
- `app/src/main/java/.../logic/IngredientMatcher.java` — strict ingredient and quantity matching
- `app/src/main/res/layout/` — screen and row layouts
- `app/src/test/` — unit tests for missing ingredients, quantities, and unit conversion

## Database model

```text
pantry_items
  id (PK), name, quantity, unit, expiry_date

recipes
  id (PK), name, summary, steps

recipe_ingredients
  id (PK), recipe_id (FK), name, quantity, unit

app_settings
  setting_key (PK), setting_value
```

`recipes` and `recipe_ingredients` are seeded only when the local database is first created. Pantry data and preferences are written to SQLite and remain after closing the app.

## Check the core rule

`IngredientMatcher.canMake(recipe, pantry)` checks each required recipe ingredient. For each one, it normalizes the name, totals compatible pantry quantities, and rejects the recipe immediately if the amount is insufficient. It returns `true` only after every requirement passes. There is no partial-match section.

The test cases in `app/src/test` cover plural names, metric mass conversion, a missing ingredient, and an insufficient amount. Run them from Android Studio using **app > Tasks > verification > test**, or use the Gradle test task after Android Studio has created/configured the Gradle wrapper.


See `docs/REPORT_TEMPLATE.md` and `docs/DEMO_CHECKLIST.md`. Do not submit the templates unchanged.
