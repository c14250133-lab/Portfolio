import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

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

print("Preview Data:")
print(df.head())

print("\nStatistik Deskriptif:")
print(df.describe())

sns.boxplot(data=df)
plt.title("Boxplot Sebelum Outlier Removal (IQR)")
plt.xticks(rotation=45)
plt.show()

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

print("\nUkuran data sebelum outlier removal:", df.shape)
print("Ukuran data setelah outlier removal:", df_clean.shape)

sns.boxplot(data=df_clean)
plt.title("Boxplot Setelah Outlier Removal (IQR)")
plt.xticks(rotation=45)
plt.show()

from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
df_scaled = df_clean.copy()
df_scaled[df.columns] = scaler.fit_transform(df_clean[df.columns])

print("\nPreview data setelah normalisasi:")
print(df_scaled.head())

X = df_scaled.drop("aesthetic_score", axis=1)
y = df_scaled["aesthetic_score"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print("\nJumlah data training:", X_train.shape)
print("Jumlah data testing:", X_test.shape)

model = LinearRegression()
model.fit(X_train, y_train)

print("\nKoefisien Model:", model.coef_)
print("Intercept:", model.intercept_)

from sklearn.metrics import mean_squared_error, r2_score
y_pred = model.predict(X_test)

print("\nMSE:", mean_squared_error(y_test, y_pred))
print("R2 Score:", r2_score(y_test, y_pred))

plt.scatter(y_test, y_pred, alpha=0.7)
plt.xlabel("Aktual Estetika")
plt.ylabel("Prediksi Estetika")
plt.title("Prediksi vs Aktual Estetika (Linear Regression)")
plt.show()
