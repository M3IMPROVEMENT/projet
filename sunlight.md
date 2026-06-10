from pathlib import Path

print(list(Path(".").rglob("master_companies.xlsx")))
