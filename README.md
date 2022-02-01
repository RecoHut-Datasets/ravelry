
## Tree

```
.
└── [ 78K]  ravelry.parquet.snappy

  82K used in 0 directories, 1 file
```

## Processing

```python
import pandas as pd

df = pd.read_csv('https://github.com/RecoHut-Datasets/ravelry/raw/v1/ravelry_interactions.csv')
df_drop_nans = df[['user', 'pattern_id', 'rating']].dropna(subset = ['rating'])
df_replace_nans = df[['user', 'pattern_id', 'rating', 'average_rating']]
rating_replace_nans = df_replace_nans['rating'].fillna(df_replace_nans['average_rating'])
df_replace_nans['rating'] = rating_replace_nans
df_replace_nans.drop(columns = 'average_rating', inplace = True)

print(df_replace_nans.info())
print(df_replace_nans.columns)

df_replace_nans.to_parquet('ravelry.parquet.snappy', compression='snappy')
```