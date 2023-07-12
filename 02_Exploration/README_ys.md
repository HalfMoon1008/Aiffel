
<img width="500" alt="result" src="https://github.com/HalfMoon1008/Aiffel/assets/86039672/4465f401-9c24-4905-9c7b-fd993bdd0060">

# AIFFEL Campus Online 5th Code Peer Review Templete
- 코더 : 윤상현
- 리뷰어 : 어윤석


# PRT(PeerReviewTemplate) 
각 항목을 스스로 확인하고 토의하여 작성한 코드에 적용합니다.

- [X] 코드가 정상적으로 동작하고 주어진 문제를 해결했나요?
  > 정상적으로 동작하고 주어진 문제를 해결하였습니다.
- [X] 주석을 보고 작성자의 코드가 이해되었나요?
  > 상세 내용이 주석으로 잘 설명되어 있어 코드를 이해하는데 많은 도움이 되었습니다.
- [X] 코드가 에러를 유발할 가능성이 없나요?
  > 
- [X] 코드 작성자가 코드를 제대로 이해하고 작성했나요?
  > 다양한 시각화 라이브러리로 데이터를 분석하고  
  그 분석을 바탕으로 학습을 위해 새롭게 feature 구성을 하였습니다.  
- [X] 코드가 간결한가요?
  > 내용이 많아 다소 어려운 부분이 있지만  
  상세한 주석과 함께 필요한 코드들로 구현된 것 같습니다.

# 코드 리뷰
### 상관계수 분석에 heatmap을 활용하는 부분이 유용하게 느껴졌습니다.
```python
# 상관관계 다시 분석

cor_abs = abs(df_train.corr(method='spearman')) 
cor_cols = cor_abs.nlargest(n=10, columns='price').index # price과 correlation이 높은 column 10개 뽑기(내림차순)
# spearman coefficient matrix
cor = np.array(sp.stats.spearmanr(df_train[cor_cols].values))[0] # 10 x 10
print(cor_cols.values)

plt.figure(figsize=(10,10))
sns.set(font_scale=1.25)
sns.heatmap(cor, fmt='.2f', annot=True, square=True , annot_kws={'size' : 8} ,xticklabels=cor_cols.values, yticklabels=cor_cols.values, cmap = "Blues")
```
### 분석 데이터를 바탕으로 새로운 feature 를 만드는 부분이 인상적이었습니다.
```python
df['total_rooms'] = df['bedrooms'] + df['bathrooms']
# 전체 방 수 = 침실 + 화장실

df['grade_condition'] = df['grade'] * df['condition']
# 등급_상태 = 집의 등급 * 집의 전반적인 상태

df['sqft_total'] = df['sqft_living'] + df['sqft_lot']
# 전체 면적
# 피트 기준
# 주거 공간 면적 + 부지 공간 면적

df['sqft_total_size'] = df['sqft_living'] + df['sqft_lot'] + df['sqft_above'] + df['sqft_basement']
# 전체 사이즈
# 피트 기준
# 주거 면적 + 부지 면적 + 지하실 제외(차고 등등 _ 이하 차고) + 지하실 포함

df['sqft_total15'] = df['sqft_living15'] + df['sqft_lot15'] 
# 15년 기준 전체 면적
# 15년 기준 주거 면적 + 15년 기준 부지 면적

df['is_renovated'] = df['yr_renovated'] - df['yr_built']
# 재건축 _ 재건축을 안했다면 0, 했다면 지어진지 a년 뒤에 재건축
# 재건축 년도 - 지어진 년도

df['is_renovated'] = df['is_renovated'].apply(lambda x: 0 if x == 0 else 1)
# 재건축 했다면 1, 안했다면 0

df['roombybathroom'] = df['bedrooms'] / df['bathrooms']
# 방마다 있는 화장실 수
# 침실 / 화장실

df['sqft_total_by_lot'] = (df['sqft_living'] + df['sqft_above'] + df['sqft_basement'])/df['sqft_lot']
# 주거 면적 % 부지 면적
# (주거 공간 + 차고 + 지하실) / 부지 면적

qcut_count = 10
# 10(피트)을 기준으로 공간 나누기 & int화
df['qcut_long'] = pd.qcut(df['long'], qcut_count, labels=range(qcut_count))
df['qcut_lat'] = pd.qcut(df['lat'], qcut_count, labels=range(qcut_count))
df['qcut_long'] = df['qcut_long'].astype(int)
df['qcut_lat'] = df['qcut_lat'].astype(int)

df['date'] = pd.to_datetime(df['date'])
df['yearmonth'] = df['date'].dt.year*100 + df['date'].dt.month
df['date'] = df['date'].astype('int')
```

