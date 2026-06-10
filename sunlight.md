import pandas as pd

all_columns = set()

for file in files:
    try:
        df = pd.read_excel(file, nrows=5)

        for col in df.columns:
            all_columns.add(str(col).strip())

    except:
        pass

print(sorted(all_columns))
