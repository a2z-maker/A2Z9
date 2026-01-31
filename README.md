import streamlit as st
from pytrends.request import TrendReq
import pandas as pd

st.title("🔥 Top 10 Produits Tendance (Monde)")

pytrends = TrendReq(hl='en-US', tz=360)

produits = [
    "wireless earbuds",
    "smart watch",
    "air fryer",
    "phone stand",
    "gaming chair",
    "power bank",
    "led lights",
    "bluetooth speaker",
    "robot vacuum",
    "fitness tracker"
]

scores = []

for produit in produits:
    pytrends.build_payload([produit], timeframe='now 1-d', geo='')
    data = pytrends.interest_over_time()
    score = data[produit].mean() if not data.empty else 0
    scores.append(score)

df = pd.DataFrame({
    "Produit": produits,
    "Score Popularité": scores
})

df = df.sort_values(by="Score Popularité", ascending=False)

st.table(df.head(10))
