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


mapping = {

    "Société":"STE",
    "Raison Sociale":"STE",
    "Nom Société":"STE",

    "Contact":"NAME",
    "Nom":"NAME",
    "Responsable":"NAME",

    "TVA":"TVA",
    "VAT":"TVA",

    "Adresse":"ADDRESS",
    "Lieu":"ADDRESS",

    "CP":"CP",
    "Code Postal":"CP",

    "GSM":"GSM",
    "Mobile":"GSM",

    "Téléphone":"FIX",
    "Fixe":"FIX",

    "Forme Juridique":"FORME_JURIDIQUE",

    "Code Linguistique":"CODE_LINGUISTIQUE",

    "Civilité":"CIVILITE",

    "Commentaires":"COMMENTS"
}
df = pd.read_excel(files[0])
print(df.head())
print(df.columns)
