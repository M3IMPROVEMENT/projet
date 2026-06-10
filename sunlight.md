def detect_file_type(file):

    try:
        # Try reading normally
        df = pd.read_excel(file)

        cols = [str(c).lower() for c in df.columns]

        # CASE 1: headers are meaningful text
        if any("nom" in c or "client" in c or "phone" in c or "tel" in c for c in cols):
            return "TYPE_A_HEADER_OK"

        # CASE 2: headers are numbers → likely no header
        if all(isinstance(c, (int, float)) for c in df.columns):
            return "TYPE_B_NO_HEADER"

        # CASE 3: messy / unknown structure
        return "TYPE_C_MESSY"

    except:
        return "TYPE_UNKNOWN"



classified = []

for file in files:
    file_type = detect_file_type(file)

    classified.append({
        "file": file.name,
        "type": file_type
    })

import pandas as pd
df_types = pd.DataFrame(classified)

df_types.to_excel("file_classification.xlsx", index=False)

print(df_types["type"].value_counts())
