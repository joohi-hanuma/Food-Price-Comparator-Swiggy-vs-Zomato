import streamlit as st
import pandas as pd
import random
import time

# Streamlit UI config
st.set_page_config(page_title="🍴 Food Price Comparator", layout="centered")
st.title("🍲 Food Price Comparator — Swiggy vs Zomato")

# Input
food_item = st.text_input("Enter a food item (e.g., Biryani, Pizza):")

# Simulate fake data
def simulate_results(food):
    variations = [
        f"{food} Regular", f"{food} Large", f"Spicy {food}", f"{food} Combo", f"{food} with Coke",
        f"Grilled {food}", f"{food} Double", f"{food} Family Pack", f"{food} Cheese Burst", f"{food} Supreme"
    ]
    swiggy_prices = [random.randint(150, 400) for _ in variations]
    zomato_prices = [random.randint(150, 400) for _ in variations]

    def random_rating():
        return round(random.uniform(3.5, 5.0), 1)

    swiggy_data = [{
        "Platform": "Swiggy",
        "Item": item,
        "Price": f"₹{price}",
        "PriceValue": price,
        "Rating": random_rating(),
        "OrderLink": f"https://www.swiggy.com/search?query={item.replace(' ', '%20')}"
    } for item, price in zip(variations, swiggy_prices)]

    zomato_data = [{
        "Platform": "Zomato",
        "Item": item,
        "Price": f"₹{price}",
        "PriceValue": price,
        "Rating": random_rating(),
        "OrderLink": f"https://www.zomato.com/search?q={item.replace(' ', '%20')}"
    } for item, price in zip(variations, zomato_prices)]

    return swiggy_data + zomato_data

# Main Logic
if food_item:
    with st.spinner("🍳 Cooking up your deals..."):
        time.sleep(1.5)
        results = simulate_results(food_item)

    if results:
        df = pd.DataFrame(results)
        special_index = random.randint(0, len(df) - 1)  # Pick one item as Today's Special

        for i, row in df.iterrows():
            special = "🔥 Today’s Special!" if i == special_index else ""

            with st.container():
                st.markdown(f"""
                ### 🍽 {row['Item']} {special}
                - 📦 Platform: {row['Platform']}
                - 💸 Price: {row['Price']}
                - 🔗 [Order Now]({row['OrderLink']})
                """, unsafe_allow_html=True)

                if st.button(f"⭐ Show Rating for {row['Item']} ({row['Platform']})", key=f"rating_{i}"):
                    st.info(f"⭐ Rating: {row['Rating']} / 5")

        # Find best deal
        best = df.sort_values("PriceValue").iloc[0]
        st.success(f"✅ Best deal: {best['Item']} on {best['Platform']} at {best['Price']}")
        st.markdown(f"[👉 Order on {best['Platform']}]({best['OrderLink']})", unsafe_allow_html=True)
    else:
        st.warning("No results found. Try another food item.")
