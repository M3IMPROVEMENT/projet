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
