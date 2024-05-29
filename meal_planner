import streamlit as st
import sqlite3
import pandas as pd

# Initialize the SQLite database
conn = sqlite3.connect('meal_planner.db')
c = conn.cursor()

# Create a table for user preferences if it doesn't exist
c.execute('''
CREATE TABLE IF NOT EXISTS UserPreferences (
    email TEXT PRIMARY KEY,
    mealsPerDay INTEGER,
    numberOfPeople INTEGER,
    daysPerWeekCook INTEGER,
    dietaryRestrictions TEXT,
    foodDislikes TEXT,
    healthGoals TEXT,
    nutritionalGuidelines TEXT,
    cookingSkillLevel TEXT,
    favoriteCuisines TEXT,
    recipePreference TEXT,
    timeConstraints TEXT,
    workMeals TEXT,
    organicPreference TEXT,
    localStores TEXT,
    specialtyIngredients TEXT,
    ingredientSwapOption BOOLEAN,
    mealVarietyPreference BOOLEAN,
    kitchenAppliances TEXT,
    onlineToolsComfort BOOLEAN
)
''')
conn.commit()

# Function to fetch user preferences
def fetch_user_preferences(email):
    c.execute('SELECT * FROM UserPreferences WHERE email=?', (email,))
    return c.fetchone()

# Function to save user preferences
def save_user_preferences(preferences):
    c.execute('''
    INSERT INTO UserPreferences (email, mealsPerDay, numberOfPeople, daysPerWeekCook, dietaryRestrictions, foodDislikes, healthGoals, nutritionalGuidelines, cookingSkillLevel, favoriteCuisines, recipePreference, timeConstraints, workMeals, organicPreference, localStores, specialtyIngredients, ingredientSwapOption, mealVarietyPreference, kitchenAppliances, onlineToolsComfort)
    VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    ON CONFLICT(email) DO UPDATE SET
    mealsPerDay=excluded.mealsPerDay,
    numberOfPeople=excluded.numberOfPeople,
    daysPerWeekCook=excluded.daysPerWeekCook,
    dietaryRestrictions=excluded.dietaryRestrictions,
    foodDislikes=excluded.foodDislikes,
    healthGoals=excluded.healthGoals,
    nutritionalGuidelines=excluded.nutritionalGuidelines,
    cookingSkillLevel=excluded.cookingSkillLevel,
    favoriteCuisines=excluded.favoriteCuisines,
    recipePreference=excluded.recipePreference,
    timeConstraints=excluded.timeConstraints,
    workMeals=excluded.workMeals,
    organicPreference=excluded.organicPreference,
    localStores=excluded.localStores,
    specialtyIngredients=excluded.specialtyIngredients,
    ingredientSwapOption=excluded.ingredientSwapOption,
    mealVarietyPreference=excluded.mealVarietyPreference,
    kitchenAppliances=excluded.kitchenAppliances,
    onlineToolsComfort=excluded.onlineToolsComfort
    ''', preferences)
    conn.commit()

# Streamlit UI
st.title('Personalized Meal Planner')

# User Authentication
st.sidebar.header('User Authentication')
email = st.sidebar.text_input('Enter your email')
if 'authenticated' not in st.session_state:
    st.session_state['authenticated'] = False

if st.sidebar.button('Submit'):
    if email:
        st.session_state['authenticated'] = True
    else:
        st.sidebar.warning('Please enter your email')

if st.session_state['authenticated']:
    st.sidebar.success(f'Logged in as {email}')
    preferences = fetch_user_preferences(email)
    if preferences:
        st.subheader('Welcome back!')
        action = st.radio('Would you like to reorder with the same preferences or modify them?', ('Reorder', 'Modify'))
        if action == 'Reorder':
            st.success('Your meal plan will be created with your saved preferences.')
        elif action == 'Modify':
            st.subheader('Modify Your Preferences')
            meals_per_day = st.number_input('How many meals per day?', min_value=1, max_value=5, value=preferences[1])
            number_of_people = st.number_input('How many people?', min_value=1, value=preferences[2])
            days_per_week_cook = st.number_input('How many days a week do you plan to cook?', min_value=1, max_value=7, value=preferences[3])
            dietary_restrictions = st.text_input('Do you follow any specific dietary restrictions or allergies?', value=preferences[4])
            food_dislikes = st.text_input('Are there any foods you dislike or prefer to avoid?', value=preferences[5])
            health_goals = st.text_input('What are your main health goals?', value=preferences[6])
            nutritional_guidelines = st.text_input('Are you following any specific nutritional guidelines or macros?', value=preferences[7])
            cooking_skill_level = st.selectbox('How would you describe your cooking skill level?', ['Beginner', 'Intermediate', 'Advanced'], index=['Beginner', 'Intermediate', 'Advanced'].index(preferences[8]))
            favorite_cuisines = st.text_input('Do you have any favorite cuisines or types of meals?', value=preferences[9])
            recipe_preference = st.selectbox('Do you prefer simple, quick recipes or are you open to more complex meals?', ['Simple', 'Complex'], index=['Simple', 'Complex'].index(preferences[10]))
            time_constraints = st.text_input('Do you have any time constraints or busy periods that affect your cooking schedule?', value=preferences[11])
            work_meals = st.text_input('Are you looking for meals that can be easily taken to work or eaten on the go?', value=preferences[12])
            organic_preference = st.selectbox('Do you prefer organic or specific types of ingredients?', ['Yes', 'No'], index=['Yes', 'No'].index(preferences[13]))
            local_stores = st.text_input('Are there any local stores or markets you prefer for grocery shopping?', value=preferences[14])
            specialty_ingredients = st.text_input('Do you have access to specialty ingredients (e.g., grass-fed beef, wild-caught fish)?', value=preferences[15])
            ingredient_swap_option = st.selectbox('Would you like the option to swap or substitute ingredients?', ['Yes', 'No'], index=['Yes', 'No'].index(preferences[16]))
            meal_variety_preference = st.selectbox('Do you prefer having a variety of meals each week or are you okay with repeating meals?', ['Variety', 'Repeat'], index=['Variety', 'Repeat'].index(preferences[17]))
            kitchen_appliances = st.text_input('Do you have any kitchen appliances you frequently use?', value=preferences[18])
            online_tools_comfort = st.selectbox('Are you comfortable using online tools and apps for tracking your meal plans and nutrition?', ['Yes', 'No'], index=['Yes', 'No'].index(preferences[19]))
            
            if st.button('Save Preferences'):
                new_preferences = (
                    email,
                    meals_per_day,
                    number_of_people,
                    days_per_week_cook,
                    dietary_restrictions,
                    food_dislikes,
                    health_goals,
                    nutritional_guidelines,
                    cooking_skill_level,
                    favorite_cuisines,
                    recipe_preference,
                    time_constraints,
                    work_meals,
                    organic_preference,
                    local_stores,
                    specialty_ingredients,
                    ingredient_swap_option,
                    meal_variety_preference,
                    kitchen_appliances,
                    online_tools_comfort
                )
                save_user_preferences(new_preferences)
                st.success('Preferences updated successfully!')
    else:
        st.subheader('New User')
        meals_per_day = st.number_input('How many meals per day?', min_value=1, max_value=5)
        number_of_people = st.number_input('How many people?', min_value=1)
        days_per_week_cook = st.number_input('How many days a week do you plan to cook?', min_value=1, max_value=7)
        dietary_restrictions = st.text_input('Do you follow any specific dietary restrictions or allergies?')
        food_dislikes = st.text_input('Are there any foods you dislike or prefer to avoid?')
        health_goals = st.text_input('What are your main health goals?')
        nutritional_guidelines = st.text_input('Are you following any specific nutritional guidelines or macros?')
        cooking_skill_level = st.selectbox('How would you describe your cooking skill level?', ['Beginner', 'Intermediate', 'Advanced'])
        favorite_cuisines = st.text_input('Do you have any favorite cuisines or types of meals?')
        recipe_preference = st.selectbox('Do you prefer simple, quick recipes or are you open to more complex meals?', ['Simple', 'Complex'])
        time_constraints = st.text_input('Do you have any time constraints or busy periods that affect your cooking schedule?')
        work_meals = st.text_input('Are you looking for meals that can be easily taken to work or eaten on the go?')
        organic_preference = st.selectbox('Do you prefer organic or specific types of ingredients?', ['Yes', 'No'])
        local_stores = st.text_input('Are there any local stores or markets you prefer for grocery shopping?')
        specialty_ingredients = st.text_input('Do you have access to specialty ingredients (e.g., grass-fed beef, wild-caught fish)?')
        ingredient_swap_option = st.selectbox('Would you like the option to swap or substitute ingredients?', ['Yes', 'No'])
        meal_variety_preference = st.selectbox('Do you prefer having a variety of meals each week or are you okay with repeating meals?', ['Variety', 'Repeat'])
        kitchen_appliances = st.text_input('Do you have any kitchen appliances you frequently use?')
        online_tools_comfort = st.selectbox('Are you comfortable using online tools and apps for tracking your meal plans and nutrition?', ['Yes', 'No'])
        
        if st.button('Save Preferences'):
            new_preferences = (
                email,
                meals_per_day,
                number_of_people,
                days_per_week_cook,
                dietary_restrictions,
                food_dislikes,
                health_goals,
                nutritional_guidelines,
                cooking_skill_level,
                favorite_cuisines,
                recipe_preference,
                time_constraints,
                work_meals,
                organic_preference,
                local_stores,
                specialty_ingredients,
                ingredient_swap_option,
                meal_variety_preference,
                kitchen_appliances,
                online_tools_comfort
            )
            save_user_preferences(new_preferences)
            st.success('Preferences saved successfully!')
