from pathlib import Path
import pandas as pd

files = list(Path("Data").rglob("*.xlsx"))

all_data = []

for file in files:

    try:
        # detect type again (simple reuse)
        df_test = pd.read_excel(file, nrows=5)
        cols = [str(c).lower() for c in df_test.columns]

        if any("soc" in c or "tva" in c or "gsm" in c for c in cols):
            df = process_type_1(file)
        else:
            df = process_type_2(file)

        all_data.append(df)

    except Exception as e:
        print("Error:", file.name, e)

final_df = pd.concat(all_data, ignore_index=True)
