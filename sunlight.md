import pandas as pd
from pathlib import Path

files = list(Path("Data").rglob("*.xlsx"))

results = []

def classify_file(file):

    try:
        df = pd.read_excel(file, nrows=5)

        cols = [str(c).lower() for c in df.columns]

        # ✔ TYPE 1: real headers (text-based columns)
        text_headers = sum(
            any(word in c for word in ["soc", "tva", "tel", "gsm", "nom", "adresse"])
            for c in cols
        )

        if text_headers >= 2:
            return "TYPE_1_HEADER"

        # ✔ TYPE 2: looks like raw data (your case like CHEESE CAKE)
        first_row = df.iloc[0].astype(str).tolist()

        if len(first_row) >= 4:
            return "TYPE_2_NO_HEADER"

        return "TYPE_3_MESSY"

    except:
        return "TYPE_3_MESSY"


for file in files:
    t = classify_file(file)
    results.append((file.name, t))

df_types = pd.DataFrame(results, columns=["file", "type"])

print(df_types["type"].value_counts())
df_types.to_excel("file_types.xlsx", index=False)
