results = []

for file in files:
    try:
        df = pd.read_excel(file, nrows=5)

        cols = [str(c).lower() for c in df.columns]

        if any("soc" in c or "tva" in c or "tel" in c or "gsm" in c for c in cols):
            t = "TYPE_1_HEADER"

        else:
            t = "TYPE_2_OR_MESSY"

        results.append((file.name, t))

    except Exception as e:
        print("Error:", file.name, e)

import pandas as pd

df_types = pd.DataFrame(results, columns=["file", "type"])

print(df_types["type"].value_counts())
