final_df = final_df.drop_duplicates()

final_df["GSM"] = final_df["GSM"].astype(str)
final_df["TVA"] = final_df["TVA"].astype(str)
