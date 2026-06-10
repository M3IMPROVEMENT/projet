---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
Cell In[86], line 25
     21 
     22     except Exception as e:
     23         print("Error:", file.name, e)
     24 
---> 25 final_df = pd.concat(all_data, ignore_index=True)

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\core\reshape\concat.py:407, in concat(objs, axis, join, ignore_index, keys, levels, names, verify_integrity, sort, copy)
    402 else:  # pragma: no cover
    403     raise ValueError(
    404         "Only can inner (intersect) or outer (union) join the other axis"
    405     )
--> 407 objs, keys, ndims = _clean_keys_and_objs(objs, keys)
    409 if sort is lib.no_default:
    410     if axis == 0:

File ~\AppData\Local\Python\pythoncore-3.14-64\Lib\site-packages\pandas\core\reshape\concat.py:808, in _clean_keys_and_objs(objs, keys)
    805     objs = list(objs)
    807 if len(objs) == 0:
--> 808     raise ValueError("No objects to concatenate")
    810 if keys is not None:
    811     if not isinstance(keys, Index):

ValueError: No objects to concatenate
