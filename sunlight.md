import pandas as pd
from pathlib import Path

files = list(Path("Data").rglob("*.xlsx"))

print("Total files found:", len(files))

all_columns = set()

for file in files[:50]:  # test first 50
    try:
        df = pd.read_excel(file, nrows=5)

        print("Reading:", file.name)
        print(df.columns)

        for col in df.columns:
            all_columns.add(str(col).strip())

    except Exception as e:
        print("Error in:", file.name, "->", e)

print("\nALL COLUMNS FOUND:")
print(sorted(all_columns))





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


--------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\compat\_optional.py:158, in import_optional_dependency(name, extra, min_version, errors)
    157 try:
--> 158     module = importlib.import_module(name)
    159 except ImportError as err:

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\importlib\__init__.py:88, in import_module(name, package)
     87         level += 1
---> 88 return _bootstrap._gcd_import(name[level:], package, level)

File <frozen importlib._bootstrap>:1406, in _gcd_import(name, package, level)
   1404     if level > 0:
   1405         name = _resolve_name(name, package, level)
-> 1406     return _find_and_load(name, _gcd_import)

File <frozen importlib._bootstrap>:1371, in _find_and_load(name, import_)
   1369             module = sys.modules.get(name, _NEEDS_LOADING)
   1370             if module is _NEEDS_LOADING:
-> 1371                 return _find_and_load_unlocked(name, import_)
   1372 

File <frozen importlib._bootstrap>:1335, in _find_and_load_unlocked(name, import_)
   1333     spec = _find_spec(name, path)
   1334     if spec is None:
-> 1335         raise ModuleNotFoundError(f'{_ERR_MSG_PREFIX}{name!r}', name=name)
   1336     else:

ModuleNotFoundError: No module named 'openpyxl'

The above exception was the direct cause of the following exception:

ImportError                               Traceback (most recent call last)
Cell In[13], line 1
----> 1 df = pd.read_excel(files[0])
      2 print(df.head())
      3 print(df.columns)

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\io\excel\_base.py:481, in read_excel(io, sheet_name, header, names, index_col, usecols, dtype, engine, converters, true_values, false_values, skiprows, nrows, na_values, keep_default_na, na_filter, verbose, parse_dates, date_format, thousands, decimal, comment, skipfooter, storage_options, dtype_backend, engine_kwargs)
    479 if not isinstance(io, ExcelFile):
    480     should_close = True
--> 481     io = ExcelFile(
    482         io,
    483         storage_options=storage_options,
    484         engine=engine,
    485         engine_kwargs=engine_kwargs,
    486     )
    487 elif engine and engine != io.engine:
    488     raise ValueError(
    489         "Engine should not be specified when passing "
    490         "an ExcelFile - ExcelFile already has the engine set"
    491     )

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\io\excel\_base.py:1621, in ExcelFile.__init__(self, path_or_buffer, engine, storage_options, engine_kwargs)
   1618 self.engine = engine
   1619 self.storage_options = storage_options
-> 1621 self._reader = self._engines[engine](
   1622     self._io,
   1623     storage_options=storage_options,
   1624     engine_kwargs=engine_kwargs,
   1625 )

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\io\excel\_openpyxl.py:559, in OpenpyxlReader.__init__(self, filepath_or_buffer, storage_options, engine_kwargs)
    541 @doc(storage_options=_shared_docs["storage_options"])
    542 def __init__(
    543     self,
   (...)    546     engine_kwargs: dict | None = None,
    547 ) -> None:
    548     """
    549     Reader using openpyxl engine.
    550 
   (...)    557         Arbitrary keyword arguments passed to excel engine.
    558     """
--> 559     import_optional_dependency("openpyxl")
    560     super().__init__(
    561         filepath_or_buffer,
    562         storage_options=storage_options,
    563         engine_kwargs=engine_kwargs,
    564     )

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\compat\_optional.py:161, in import_optional_dependency(name, extra, min_version, errors)
    159 except ImportError as err:
    160     if errors == "raise":
--> 161         raise ImportError(msg) from err
    162     return None
    164 # Handle submodules: if we have submodule, grab parent module from sys.modules

ImportError: `Import openpyxl` failed.  Use pip or conda to install the openpyxl package.
