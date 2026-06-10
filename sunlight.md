import pandas as pd
from pathlib import Path

files = list(Path("Data").rglob("*.xlsx"))

def classify_file(file):

    try:
        df = pd.read_excel(file, nrows=5)

        cols = [str(c).lower() for c in df.columns]

        # TYPE 1 → headers exist (keywords detected)
        if any("soc" in c or "tva" in c or "tel" in c or "gsm" in c for c in cols):
            return "TYPE_1_HEADER"

        # TYPE 2 → columns are NOT meaningful (likely data values)
        if all(isinstance(c, (int, float)) for c in df.columns) or len(cols) > 0:
            # extra check: first cell looks like company name
            first_row = df.iloc[0].astype(str).tolist()

            if any("sa" in str(first_row[0]).lower() or len(first_row) > 3):
                return "TYPE_2_NO_HEADER"

        return "TYPE_3_MESSY"

    except:
        return "TYPE_3_MESSY"


results = []

for file in files:
    t = classify_file(file)
    results.append((file.name, t))

import pandas as pd

df_types = pd.DataFrame(results, columns=["file", "type"])

print(df_types["type"].value_counts())

df_types.to_excel("file_types.xlsx", index=False)
