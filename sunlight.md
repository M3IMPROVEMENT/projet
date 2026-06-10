import pandas as pd
from pathlib import Path 
!pip install openpyxl
import openpyxl
files =list(Path(r"C:\Users\Sunlight\Desktop\DOSSIE 09-06-2026 - Copie\3-AB-12-08-2024-TR").rglob("*.xlsx"))+\
       list(Path(r"C:\Users\Sunlight\Desktop\DOSSIE 09-06-2026 - Copie\Chaima 10-06-2024--TR").rglob("*.xlsx"))+\
       list(Path(r"C:\Users\Sunlight\Desktop\DOSSIE 09-06-2026 - Copie\Imane 10-06-2024--TR").rglob("*.xlsx"))
       print("Total files:",len(files))
for file in files[:20]:
    print(file.name)
       try: 
         df = pd.read_excel(file)

         print(df.columns) 
             
   except Exception as e:  
       print("Error:", file.name, ">" , e)
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
print(df_types["type"].value_counts())
COLUMNS = [ 
    "STE",
    "NAME",
    "TVA",
    "ADDRESS",
    "CP",
    "CITY",
    "GSM",
    "TELE",
    "FORME_JURIDIQUE",
    "CODE_LINGUISTIQUE",
    "CIVILITE",
    "HOUSE_NUM"
    "COMMENTS",
    "OTHER_INFO",
    "SOURCE_FILE" 
]
import pandas as pd  

def process_type_2(file):
    df = pd.read_excel(file, header=None)

    df = df.iloc[:, :6]  # keep first 6 columns

    df.columns = ["STE", "ADDRESS", "CP", "CITY", "PHONE", "TVA"]

    df["NAME"] = None
    df["GSM"] = df["PHONE"]
    df["FIX"] = None

    df["FORME_JURIDIQUE"] = None
    df["CODE_LINGUISTIQUE"] = None
    df["CIVILITE"] = None
    df["COMMENTS"] = None
    df["OTHER_INFO"] = None

    df["SOURCE_FILE"] = file.name

    return df
def process_type_1(file):
    df = pd.read_excel(file)

    df.columns = [str(c).strip().upper() for c in df.columns]

    rename_map = {
        "SOCIETE": "STE",
        "RAISON SOCIALE": "STE",
        "NOM": "NAME",
        "TVA": "TVA",
        "ADRESSE": "ADDRESS",
        "CP": "CP",
        "VILLE": "CITY",
        "GSM": "GSM",
        "TELEPHONE": "FIX"
    }

    df.rename(columns=rename_map, inplace=True)

    df["SOURCE_FILE"] = file.name

    for col in COLUMNS:
        if col not in df.columns:
            df[col] = None

    return df[COLUMNS]
from pathlib import Path
import pandas as pd

files =list(Path(r"C:\Users\Sunlight\Desktop\DOSSIE 09-06-2026 - Copie\3-AB-12-08-2024-TR").rglob("*.xlsx"))+\
       list(Path(r"C:\Users\Sunlight\Desktop\DOSSIE 09-06-2026 - Copie\Chaima 10-06-2024--TR").rglob("*.xlsx"))+\
       list(Path(r"C:\Users\Sunlight\Desktop\DOSSIE 09-06-2026 - Copie\Imane 10-06-2024--TR").rglob("*.xlsx"))

all_data = []

for file in files:

    try:
        # detect type again (simple reuse)
        df_test = pd.read_excel(file, nrows=5)
        cols = [str(c).lower() for c in df_test.columns]

        if any(("soc" in c) or ("tva" in c) or ("gsm" in c) for c in cols):
            df = process_type_1(file)
        else:
            df = process_type_2(file)

        all_data.append(df)

    except Exception as e:
        print("Error:", file.name, "->" , e)
        
print("processed files:", len(all_data))

final_df = pd.concat(all_data, ignore_index=True)
final_df = final_df.drop_duplicates()

final_df["GSM"] = final_df["GSM"].astype(str)
final_df["TVA"] = final_df["TVA"].astype(str)
final_df.to_excel("master_companies.xlsx", index=False)
print("DONE")
import pandas as pd
df = pd.read_excel("master_companies.xlsx")
df.head()
