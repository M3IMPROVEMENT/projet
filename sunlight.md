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
