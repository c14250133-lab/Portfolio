import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
from sklearn.metrics import mean_squared_error, r2_score

st.title("🎨 Analisis Estetika Visual dengan Regresi Linear")

np.random.seed(42)
n = 150
df = pd.DataFrame({
    "num_objects": np.random.randint(1, 50, n),
    "lighting_intensity": np.random.uniform(0.1, 5, n),
    "color_saturation": np.random.uniform(0.2, 1.0, n),
    "render_time": np.random.uniform(5, 120, n),
    "frames": np.random.randint(1, 500, n),
    "aesthetic_score": np.random.uniform(0, 10, n)
})

st.subheader("📊 Data Awal")
st.dataframe(df.head())

def remove_outliers_iqr(data, column):
    Q1 = data[column].quantile(0.25)
    Q3 = data[column].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    return data[(data[column] >= lower) & (data[column] <= upper)]

df_clean = df.copy()
for col in df.columns:
    df_clean = remove_outliers_iqr(df_clean, col)

st.subheader("📦 Boxplot Setelah Outlier Removal")
fig = px.box(df_clean)
st.plotly_chart(fig)

scaler = MinMaxScaler()
df_scaled = df_clean.copy()
df_scaled[df.columns] = scaler.fit_transform(df_clean[df.columns])

X = df_scaled.drop("aesthetic_score", axis=1)
y = df_scaled["aesthetic_score"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

st.subheader("📈 Evaluasi Model")
st.write("MSE:", mean_squared_error(y_test, y_pred))
st.write("R² Score:", r2_score(y_test, y_pred))

fig2 = px.scatter(x=y_test, y=y_pred, labels={"x": "Aktual", "y": "Prediksi"}, title="Prediksi vs Aktual")
fig2.add_shape(type="line", x0=0, y0=0, x1=1, y1=1, line=dict(color="red", dash="dash"))
st.plotly_chart(fig2)
