# Pandas Cookbook 

### Pandas 기초

* 본 책의 자료는 git을 통해서 다운받으실 수 있습니다. 

  * https://github.com/PacktPublishing/Pandas-Cookbook.git

* 프로젝트를 진행하기 위해서 사용되는 기본적인 라이브러리를 설치해야 합니다.

  * jupyter, numpy, pandas etc... 

* 이번 장에서는 Series와 DataFrame의 데이터 구조를 확인하여 pandas의 기초를 이해하는 것에 있습니다. 

* Series와 DataFrame의 각 구성요소와 pandas 데이터의 각 열을 단일 데이터 형식만 저장한다는 사실을 기억합니다. 


#### DataFrame 해부 

* pandas의 DataFrame를 출력하면 인덱스, 열, 데이터로 이루어진 표가 출력됩니다. 

  ```python
  import pandas as pd
  import numpy as np
  pd.set_option('max_columns', 8, 'max_rows', 10)
  movie = pd.read_csv('data/movie.csv')
  movie.head()
  ```

* movie.head()를 통해 처음 처음 5줄을 출력합니다. 

* pandas는 read_csv 함수를 사용해서 데이터를 디스크로부터 메모리로 읽어드리고 DataFrame으로 로드합니다. 

* 통상 인덱스 레이블과 열 이름은 각각 개벽적인 인덱스나 열의 구성원을 의미하며, 인덱스란, 전체 인덱스 레이블을 통칭하며, 열이한 전체 열 이름을 통칭합니다. 

* 열과 인덱스를 통틀어 축이라고 표현합니다. 

* pandas는 누락된 값을 표기하기 위해서 NaN을 사용합니다. 

   

#### DataFrame의 주요구성 요소 사용 

* 인덱스, 열, 데이터를 각각 개별변수로 뽑아내고 열과 인덱스가 어떻게 동일한 객체의 속성을 상속했는지 확인합니다. 

  ```python
  columns = movie.columns
  columns = movie.index
  data = movie.values
  ```

  ```python
  columns
  columns
  data
  type(index)
  type(columns)
  type(data)
  issubclass(pd.RangeIndex, pd.Index)
  ```

* 구성 요소의 데이터 형식을 표시하며, 형식 이름은 출력의 마지막 점 다음에 나타난 단어입니다. 



#### 데이터 형식 이해하기 

* 데이터는 연속데이터, 범주 데이터로 나눌 수 있습니다. 연속 데이터는 항상 같은 척도의 한 종류를 나타내며, 무한 가지의 값을 가집니다. 

* 범주 데이터는 이산이고, 한정된 수의 값을 뜻합니다. 

* pandas는 데이터를 연속이나 범주형으로 광범위하게 분류하지 않으며, 서로 구분되는 데이터 형식에 대해 세밀하게 정의를 내리고 있습니다. 

  ```python
  movie.get_dtype_counts()
  
  float64    13
  int64       3
  object     12
  dtype: int64
  ```

* 위의 메서드를 통해서 각 형식별로의 개수를 구할수 있습니다. 



#### 데이터 단일 열을 Series로 선택하기 

*  Series는 DataFrame의 단일 열로 1차원 데이터고, 인덱스와 데이터로만 이루어져 있습니다. 

  ```python
  movie['director_name']
  movie.director_name
  type(movie['director_name'])
  
  director = movie['director_name'] # save Series to variable
  director.name
  director.to_frame().head()
  ```

* 파이썬은 데이터를 담기 위해서 리스트, 튜플, 딕셔너리와 같은 내장 객체가 여러개 있습니다. 

* 이 객체는 모두 인덱싱 연산자를 사용해 데이터를 선택할 수 있으며, 데이터 선택을 위한 주된 도구로 인덱싱을 사용합니다. 

* Series의 시각적 표현은 데이터릐 단일 열을 표시합니다. 



#### 인덱스를 의미 있게 만들기 

* DataFrame의 인덱스는 각 행의 레이블을 제공하며, 명시적인 인덱스를 제공하지 않으면 RangeIndex가 생성되어 n-1의 정수를 부여합니다. (n은 전체 행의 개수)

* 이러한 의미 없는 인덱스를 의미있는 영화 제목으로 변경하는 방법을 살펴봅니다. 

  ```python
  movie2.reset_index()
  
  movie = pd.read_csv('data/movie.csv', index_col='movie_title')
  
  idx_rename = {'Avatar':'Ratava', 'Spectre': 'Ertceps'} 
  col_rename = {'director_name':'Director Name', 
                'num_critic_for_reviews': 'Critical Reviews'} 
                
  movie.rename(index=idx_rename, columns=col_rename).head()
  ```

* DataFrame의 rename 메서드는 예전값을 새로운 값으로 매핑하는 딕셔너리를 입력으로 받습니다. 

* 딕셔너리를 rename 메서드에 전달하고 그 결과를 새로운 변수에 저장합니다. 

* 다음 방법은 열과 행의 속성을 파이썬 리스트에 바로 지정할 수 있는 방법을 나타냅니다. 단 이 방법은 리스트가 행과 열의 레이블과 동일한 개수의 원소를 가지고 있을 때만 동작합니다. 

  ```python
  movie = pd.read_csv('data/movie.csv', index_col='movie_title')
  index = movie.index
  columns = movie.columns
  
  index_list = index.tolist()
  column_list = columns.tolist()
  
  index_list[0] = 'Ratava'
  index_list[2] = 'Ratava'
  column_list[1] = 'Director Name'
  column_list[2] = 'Critical Reviews'
  
  print(index_list[:5])
  print(column_list)
  movie.index = index_list
  movie.columns = column_list
  movie.head()
  ```


#### 열의 생성과 삭제 

* 데이터를 분석하는 동안 새로운 변수를 사용하기 위해 열을 추가하거나 삭제하는 일이 빈번하게 발생합니다. 

* 다음 방법은 이러한 방법을 실습을 통해서 나타내고 있습니다. 

  ```python
  movie['actor_director_facebook_likes'] = (movie['actor_1_facebook_likes'] + 
                                                movie['actor_2_facebook_likes'] + 
                                                movie['actor_3_facebook_likes'] + 
                                                movie['director_facebook_likes'])
  ```

  ```python
  movie['actor_director_facebook_likes'].isnull().sum()
  ```

* 페이스북의 좋아요를 가지고 있는 값을 가지고 있는 멸 개의 열을 배우, 감독 각각의 페이스북 좋아요를 더해서 새로운 열 하나를 만들고 저장했습니다. 

* 수치 데이터 열들이 서로 합산될 때 누락값이 있으면 pandas는 디폴트 값 0으로 취급합니다. 

* 전체 행의 원소가 누락값일 때는 pandas는 누락값으로 유지함으로 모두 0으로 채우는 작업을 진행합니다. 

  ```python
  movie['actor_director_facebook_likes'] = movie['actor_director_facebook_likes'].fillna(0)
  ```


* 이번 데이터에서 값을 검증하기 위해서 다음과 같은 방법으로 데이터를 검증했으나, 검증이 실패하였으므로 해당 열을 삭제하고 다시 새로운 열을 생성합니다 .

  ```python
  movie['is_cast_likes_more'] = (movie['cast_total_facebook_likes'] >= 
                                    movie['actor_director_facebook_likes'])
  movie['is_cast_likes_more'].all()
  movie = movie.drop('actor_director_facebook_likes', axis='columns')
  
  
  movie['actor_total_facebook_likes'] = (movie['actor_1_facebook_likes'] + 
                                         movie['actor_2_facebook_likes'] + 
                                         movie['actor_3_facebook_likes'])
  
  movie['actor_total_facebook_likes'] = movie['actor_total_facebook_likes'].fillna(0)
  movie['is_cast_likes_more'] = movie['cast_total_facebook_likes'] >= \
                                    movie['actor_total_facebook_likes']
      
  movie['is_cast_likes_more'].all()
  ```

* 데이터 검증이 완료되었으므로 비율을 계산하고 열의 min과 max과 0과 1 사이가 맞는지 확인합니다. 

  ```python
  movie['pct_actor_cast_like'] = (movie['actor_total_facebook_likes'] / 
                                  movie['cast_total_facebook_likes'])
  movie['pct_actor_cast_like'].min(), movie['pct_actor_cast_like'].max() 
  ```

* 마지막으로 해당 열을 Series로 출력합니다. 먼저 인덱스를 영화 제목으로 설정하여 각 값을 식별할 수 있도록 합니다. 