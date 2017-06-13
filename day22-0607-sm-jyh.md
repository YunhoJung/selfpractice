# Excuting querysets

## Making queries
데이터 모델을 만들면 Django는 자동으로 객체를 생성, 검색, 업데이트 및 삭제할 수있는 데이터베이스 추상화 API를 제공한다.

### Creating objects
Django는 Python 객체에서 데이터베이스 테이블 데이터를 직관적인 시스템을 통해 나타낸다. 모델 클래스는 데이터베이스 테이블을 나타내며 클래스의 인스턴스는 데이터베이스 테이블의 특정 레코드를 나타냅니다. 객체를 생성하려면 키워드 인자를 사용하여 모델 클래스에 인스턴스를 생성 한 다음 save ()를 호출하여 객체를 데이터베이스에 저장한다.

```python
>>> from blog.models import Blog
>>> b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
>>> b.save()
```

### Saving changes to objects
이미 데이터베이스에 있는 객체의 변경사항을 저장하려면 save()를 호출하면 된다.

*Django는 명시 적으로 save ()를 호출 할 때까지 데이터베이스에 접근하지 않는다*

### Saving Foreignkey and ManytoManyField fields
- ForeignKey 필드를 업데이트하는 것은 정상 필드를 저장하는 것과 똑같은 방식으로 작동한다. 문제의 필드에 올바른 유형의 객체를 할당하기 만하면 된다.

```python
>>> from blog.models import Entry
>>> entry = Entry.objects.get(pk=1)
>>> cheese_blog = Blog.objects.get(name="Cheddar Talk")
>>> entry.blog = cheese_blog
>>> entry.save()
```
- ManyToManyField는 필드에 add () 메소드를 사용하여 관계에 레코드를 추가한다.

```python
>>> from blog.models import Author
>>> joe = Author.objects.create(name="Joe")
>>> entry.authors.add(joe)
```

*한 번에 ManyToManyField에 여러 레코드를 추가하려면 다음과 같이 add () 호출에 여러 인수를 포함시킨다.*

``` python
>>> john = Author.objects.create(name="John")
>>> paul = Author.objects.create(name="Paul")
>>> george = Author.objects.create(name="George")
>>> ringo = Author.objects.create(name="Ringo")
>>> entry.authors.add(john, paul, george, ringo)
```

## Retrieving objects
- 데이터베이스에서 객체를 검색하려면 모델 클래스의 Manager를 통해 QuerySet을 생성하면 된다.
- QuerySet는 데이터베이스의 객체 컬렉션을 나타낸다. 0 개, 하나 또는 여러 개의 필터를 가질 수 있다. 
- 필터는 주어진 매개 변수를 기반으로 쿼리 결과의 범위를 좁힌다. SQL 용어에서 QuerySet은 SELECT 문과 같으며 필터는 WHERE 또는 LIMIT와 같은 제한 절이다. 
- 모델의 Manager를 사용하여 QuerySet을 얻는다. 각 모델에는 최소한 하나의 Manager가 있으며 기본적으로 객체라고 한다.

> Manager는 모델 인스턴스가 아닌 모델 클래스를 통해서만 액세스 할 수 있으므로 "테이블 수준"작업과 "레코드 수준"작업을 구분할 수 있다.

### Retrieving 'all' objects
all () 메서드는 데이터베이스에있는 모든 객체의 QuerySet을 반환한다.

```python
>>> all_entries = Entry.objects.all()
```
### Retrieving specific objects with filters
전체 개체 집합에서 하위 집합만 선택해야 될 때가 있다. QuerySet을 세분화하는 가장 일반적인 두 가지 방법은 다음과 같다.

#### filter(**kwargs)
- 지정된 검색 매개 변수와 일치하는 객체가 포함 된 새 QuerySet을 반환한다.

#### exclude(**kwargs)
- 지정된 조회 매개 변수와 일치하지 않는 객체가 포함 된 새 QuerySet을 반환한다.

```python
Entry.objects.filter(pub_date__year=2006)
```

*Chaining filters*
QuerySet을 지정하는 결과는 QuerySet 그 자체이므로 상세 검색을 함께 연결할 수 있다.

#### Filtered QuerySets are unique
QuerySet을 구체화 할 때마다 이전 QuerySet에 구애받지 않는 새로운 QuerySet을 얻게 된다. 각 구체화 과정을 통해 별개의 고유한 QuerySet을 생성하고 이는 저장되고 사용되며 재사용 될 수있다. 독립적이다.

*QuerySet 생성은 어떤 데이터베이스 활동도 포함하지 않는다. 계속 필터를 함께 쌓을 수 있으며, 장고는 QuerySet이 평가 될 때까지 실제로 쿼리를 실행하지 않는다.*

```python
>>> q = Entry.objects.filter(headline__startswith="What")
>>> q = q.filter(pub_date__lte=datetime.date.today())
>>> q = q.exclude(body_text__icontains="food")
>>> print(q)
```


>위 코드에서는 마지막 행 (print (q))에서 한 번만 데이터베이스에 도달한다. 일반적으로 QuerySet의 결과는 사용자가 요청할 때까지 데이터베이스에서 가져 오지 않는다. 사용자가 요청하게 되면 QuerySet은 데이터베이스에 액세스하여 평가된다. 

### Retrieving a single object with 'get()'
filter()는 단일 객체가 쿼리와 일치하는 경우에도 항상 QuerySet을 제공한다. 이 경우 단일 요소를 포함하는 QuerySet이 된다. 쿼리와 일치하는 객체가 하나 뿐인 경우 객체를 직접 반환하는 Manager에서 get () 메서드를 사용할 수 있다.

```python
>>> one_entry = Entry.objects.get(pk=1)
```

>filter()와 마찬가지로 get()은 모든 쿼리 표현식을 사용할 수 있다. get()을 사용하는 것과 [0] 슬라이스로 filter()를 사용하는 것의 차이점에 유의해야 한다. 쿼리와 일치하는 결과가 없으면 get()은 DoesNotExist 예외를 발생시킨다. 이 예외는 쿼리가 수행되는 모델 클래스의 속성입니다. 위 코드에서 기본 키가 1 인 Entry 객체가 없으면 Django는 Entry.DoesNotExist를 발생시킵니다. 또한 장고에서 하나 이상의 아이템이 get() 쿼리와 일치하면 MultipleObjectReturned 오류가 발생한다. 이것은 모델 클래스 자체의 특성이다.

### Other Queryset method
대부분 데이터베이스에서 객체를 검색해야 할 때 all(), get(), filter() 및 exclude()를 사용한다. 그러나 이것이 전부가 아니다.. 다른 다양한 QuerySet 메소드의 전체 목록은 QuerySet API Reference를 참조하자.

### Limiting Querysets
Python의 배열 슬라이스 구문의 서브 세트를 사용하여 QuerySet을 특정 수의 결과로 제한할 수 있다. 이것은 SQL의 LIMIT 및 OFFSET 절과 동일하다.

```python
>>> Entry.objects.all()[5:10]
```
>이것은 6번째부터 10번째까지의 객체를 반환한다.(OFFSET 5 LIMIT 5)

*역슬라이스 구문은 (i.e. Entry.objects.all()[-1]) 지원되지 않는다.*

일반적으로 QuerySet을 슬라이스하면 새 QuerySet가 반환된다. 쿼리를 평가하지는 않는다. 하지만 예외가 있다. <strong>파이썬 슬라이스 구문의 "단계"매개 변수를 사용하는 경우이다.</strong> 예를 들어 첫 번째에서 10 번째까지를 2단계로 건너뛰는 객체 목록을 반환하기 위해 실제로 쿼리를 실행한다.

목록이 아닌 단일 객체를 검색하려면 (예 : SELECT foo FROM bar LIMIT 1) 슬라이스 대신 간단한 색인을 사용하는게 좋다.

### Field lookups (필드 조회)
필드 조회는 SQL WHERE 절의 덩어리를 지정하는 방법이다. 그것들은 QuerySet 메소드 filter(), exclude() 및 get()에 대한 키워드 인수로 지정된다. 기본 검색 키워드 인수는 field__lookuptype = value 형식을 취한다. (이중 밑줄-double underscore)

```python
>>> Entry.objects.filter(pub_date__lte='2006-01-01')
```
은 SQL 구문으로 이렇게 해석된다.

```SQL
SELECT * FROM blog_entry WHERE pub_date <= '2006-01-01';
```

찾아보기에 지정된 필드는 모델 필드의 이름이어야 한다. 그러나 한 가지 예외가 있다. ForeignKey의 경우 _id 접미사로 필드 이름을 지정할 수 있다. 이 경우, value 매개 변수에는 외부 모델의 기본 키의 pk값이 포함될 것으로 예상된다.

```python
>>> Entry.objects.filter(blog_id=4)
```
아래 두 코드는 동일하다.

```python
>>> Blog.objects.get(id__exact=14)  # Explicit form
>>> Blog.objects.get(id=14)         # __exact is implied
```
위의 exact 필드 조회 구문은

```SQL
SELECT ... WHERE headline = 'Cat bites dog';
```
위의 SQL 구문으로 해석된다.

```python
>>> Blog.objects.get(name__iexact="beatles blog") # 대소문자 구분 X
```
다음 파이썬 구문은

```python
>>> Entry.objects.get(headline__contains='Lennon') # 포함된 쿼리를 가져온다.
```
다음 SQL 구문으로 해석된다.

```SQL
SELECT ... WHERE headline LIKE '%Lennon%';
```

### Lookups that span relationships
Django는 백그라운드에서 자동으로 SQL JOIN을 처리하면서 조회에서 관계를 따른다. 관계를 확장하려면 원하는 필드를 찾을 때까지 두 모델 모두에서 관련 필드의 필드 이름을 두 개의 밑줄로 구분하여 사용하면 된다. 이 예제는 이름이 'Beatles Blog'인 블로그가있는 모든 Entry 객체를 검색한다.

```python
>>> Entry.objects.filter(blog__name='Beatles Blog')
```
확장은 원하는만큼 가능하다. 거꾸로도 작동한다. 역참조를 하기 위해서는 모델의 소문자 이름을 사용하면 된다. 이 예에서는 헤드 라인에 'Lennon'이 포함 된 항목이 하나 이상있는 모든 Blog 객체를 검색한다.

```python
>>> Blog.objects.filter(entry__headline__contains='Lennon')
```
여러 관계를 거쳐 필터링하고 중간 모델 중 하나가 필터 조건을 만족하는 값을 가지지 않는다면 Django는 이것을 공백 (모든 값은 NULL 임)으로 처리하지만 유효하다. 이 모든 것은 오류가 발생하지 않는다는 것을 의미한다.

(관련 작성자 모델이있는 경우) 항목과 연관된 작성자가없는 경우 누락 된 작성자로 인해 오류가 발생하지 않고 이름이 첨부 된 것처럼 처리된다. 혼란 스러울 수있는 유일한 경우는 isnull을 사용하는 경우입니다. 그러므로:

```python
Blog.objects.filter(entry__authors__name__isnull=True)
```
작성자에 빈 이름이있는 Blog 객체와 항목에 빈 작성자가있는 Blog 객체를 반환한다. 그 후자의 물건을 원하지 않는다면, 다음과 같이 쓸 수 있다.

```python
Blog.objects.filter(entry__authors__isnull=False, entry__authors__name__isnull=True)
```

#### Spanning multi-valued relationships(다중값 관계 확장하기)

```python
In [43]: Blog.objects.filter(Q(entry__headline__contains='Lennon') & Q(entry__pub_date__year=2017))
Out[43]: <QuerySet [<Blog: This blog has (Lennon, 2017) and (Other, 2008)>]>
In [44]: Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2017)
Out[44]: <QuerySet [<Blog: This blog has (Lennon, 2017) and (Other, 2008)>, <Blog: This blog has (Lennon, 2017) and (Other, 2008)>]>
In [45]: Blog.objects.filter(entry__headline__contains='Lennon', entry__pub_date__year=2017)
Out[45]: <QuerySet [<Blog: This blog has (Lennon, 2017) and (Other, 2008)>]>
# 3가지의 차이 인지하기 !
```
```python
In [47]: Blog.objects.exclude(entry__headline__contains='Lennon', entry__pub_date__year=2008)
Out[47]: <QuerySet [<Blog: This blog has (Lennon, 2017) and (Other, 2008)>]>
In [48]: Blog.objects.exclude(entry__headline__contains='Lennon').exclude(entry__pub_date__year=2008)
Out[48]: <QuerySet []>
In [51]: Blog.objects.exclude(entry__in=Entry.objects.filter(headline__contains='Lennon', pub_date__year=2008))
Out[51]: <QuerySet [<Blog: This blog has (Lennon, 2017) and (Other, 2008)>]>
```

>A N B -> filter<br>
~(A N B) -> exclude 계산<br>
~A U ~B -> exclude 실제 값

### Filters can reference fields on the model
지금까지는 모델 필드의 값과 상수를 비교하는 필터를 만들었다. 그러나 모델 필드의 값을 같은 모델의 다른 필드와 비교해야 할 경우가 있다. Django는 이러한 비교를 허용하는 F 식을 제공한다. F ()의 인스턴스는 쿼리 내의 모델 필드에 대한 참조로 작동한다. 그런 다음 이러한 참조를 쿼리 필터에 사용하여 동일한 모델 인스턴스의 두 개의 다른 필드 값을 비교할 수 있다. 예를 들어 pingback보다 많은 코멘트가 있는 모든 블로그 항목의 목록을 찾으려면 핑백 수를 참조하는 F () 객체를 생성하고 쿼리에서 해당 F () 객체를 사용한다.

```python
>>> from django.db.models import F
>>> Entry.objects.filter(n_comments__gt=F('n_pingbacks'))
```

Django는 상수와 다른 F () 객체와 함께 F () 객체를 사용하여 덧셈, 뺄셈, 곱셈, 나눗셈, 모듈러스 및 지수 연산을 지원한다. pingback보다 2 배 많은 주석이있는 모든 블로그 항목을 찾으려면 다음과 같이 쿼리를 수정하면 된다.

```python
>>> Entry.objects.filter(n_comments__gt=F('n_pingbacks') * 2)
```

엔트리의 등급이 핑백과 코멘트 합보다 작은 모든 엔트리를 찾으려면, 다음과 같은 쿼리를 발행 행하면 될 것이다.

```python
>>> Entry.objects.filter(rating__lt=F('n_comments') + F('n_pingbacks'))
```
이중 밑줄 표기법을 사용하여 F () 객체의 관계를 확장 할 수도 있다. 이중 밑줄이있는 F () 객체는 관련 객체에 액세스하는 데 필요한 접점을 도입한다. 예를 들어 저자 이름이 블로그 이름과 동일한 모든 항목을 검색하려면 다음과 같은 쿼리를 발행 할 수 있다.

```python
>>> Entry.objects.filter(authors__name=F('blog__name'))
```
날짜 및 날짜 / 시간 필드의 경우 timedelta 객체를 더하거나 뺄 수 있다. 다음은 게시되고 3 일 이상 지난 뒤, 수정 된 모든 항목을 반환한다.

```python
>>> from datetime import timedelta
>>> Entry.objects.filter(mod_date__gt=F('pub_date') + timedelta(days=3))
```

F () 객체는 .bitand (), .bitor (), .bitrightshift () 및 .bitleftshift ()에 의한 비트 연산을 지원한다

```python
>>> F('somefield').bitand(16)
```

### The pk lookup shortcut
편의상 Django는 "기본 키"를 나타내는 pk 조회 바로 가기를 제공한다. 예제 블로그 모델에서, pk는 id 필드이므로 세 명령문은 동일하다.

```python
>>> Blog.objects.get(id__exact=14) # Explicit form
>>> Blog.objects.get(id=14) # __exact is implied
>>> Blog.objects.get(pk=14) # pk implies id__exact
```

pk의 사용은 __exact 쿼리에만 국한되지 않는다. 쿼리 용어를 pk와 결합하여 모델의 기본 키에 대한 쿼리를 수행 할 수 있다.

```python
# Get blogs entries with id 1, 4 and 7
>>> Blog.objects.filter(pk__in=[1,4,7])

# Get all blog entries with id > 14
>>> Blog.objects.filter(pk__gt=14)
```
pk 조회는 조인 전체에서 작동한다. 예를 들어, 다음 세 문장은 동일하다.

```python
>>> Entry.objects.filter(blog__id__exact=3) # Explicit form
>>> Entry.objects.filter(blog__id=3)        # __exact is implied
>>> Entry.objects.filter(blog__pk=3)        # __pk implies __id__exact
```

### Escaping percent signs and underscores in LIKE statements
LIKE 문 (iexact, contains, icontains, startswith, endswith 및 iendswith)과 같은 필드 조회는, LIKE 문에 사용 된 두 개의 특수 문자 (백분율 기호 및 밑줄)를 자동으로 빠져나가게 한다. (LIKE 문에서 백분율 기호는 여러 문자 와일드 카드를 나타내며 밑줄 문자는 한 문자 와일드 카드를 나타낸다다.) 즉, 직관적으로 작동해야하므로 추상화가 누출되지 않습니다. 예를 들어 퍼센트 기호가 포함 된 모든 항목을 검색하려면 백분율 기호를 다른 문자로 사용하십시오.

```python
>>> Entry.objects.filter(headline__contains='%')
```
장고는 당신을 인용합니다. SQL 결과는 다음과 같이 보입니다.

```sql
SELECT ... WHERE headline LIKE '%\%%';
```
### Caching and QuerySets
각 QuerySet에는 데이터베이스 액세스를 최소화하는 캐시가 있다. 새로 생성 된 QuerySet에서 캐시는 비어 있다. QuerySet이 처음 평가 될 때 - 이때, 데이터베이스 쿼리가 발생한다 - Django는 쿼리 결과를 QuerySet의 캐시에 저장하고 명시적으로 요청 된 결과를 반환한다 (예 : QuerySet이 반복되는 경우 다음 요소) . QuerySet의 후속 평가는 캐시 된 결과를 다시 사용한다. QuerySets를 올바르게 사용하지 않으면 쿼리가 제대로 작동하지 않을 수 있다. 예를 들어, 다음은 두 개의 QuerySets을 만들고 평가 한 다음 버린다.

```python
>>> print([e.headline for e in Entry.objects.all()])
>>> print([e.pub_date for e in Entry.objects.all()])
```
즉, 동일한 데이터베이스 쿼리가 두 번 실행되어 데이터베이스로드가 두 배로 늘어난다. 또한 항목이 두 요청 사이의 두번 쨰 분할 때에 추가되거나 삭제되었을 수 있기 때문에, 두 목록에 동일한 데이터베이스 레코드가 포함되지 않을 수도 있다. 이 문제를 피하려면 단순히 QuerySet을 저장하고 다시 사용하면 된다.

```python
>>> queryset = Entry.objects.all()
>>> print([p.headline for p in queryset]) # Evaluate the query set.
>>> print([p.pub_date for p in queryset]) # Re-use the cache from the evaluation.
```
#### When QuerySets are not cached
Querysets은 결과를 항상 캐시하지는 않는다. 쿼리 세트의 일부만 평가할 때는 캐시가 검사되지만, 채워지지 않은 경우 후속 쿼리에서 반환되는 항목은 캐시되지 않는다. 특히 이는 배열 슬라이스나 인덱스를 사용하여 쿼리 세트를 제한해도 캐시가 채워지지 않음을 의미한다. 예를 들어, 쿼리 세트 객체에서 반복적으로 특정 인덱스를 얻으면 매번 데이터베이스를 쿼리한다.

```python
>>> queryset = Entry.objects.all()
>>> print(queryset[5]) # Queries the database
>>> print(queryset[5]) # Queries the database again
```
그러나 전체 쿼리 세트가 이미 평가 된 경우 캐시가 대신 확인된다.

```python
>>> [entry for entry in queryset]
>>> bool(queryset)
>>> entry in queryset
>>> list(queryset)
```
>queryset을 인쇄하면 캐시가 채워지지 않는다. 이는 __repr __ ()을 호출하면 전체 쿼리 세트의 조각만 반환되기 때문이다.

## Complex lookups with Q objects
filter() 등의 키워드 인수 쿼리는 함께 AND로 처리된다. 더 복잡한 쿼리 (예 : OR 문을 사용하는 쿼리)를 실행해야하는 경우 Q 개체를 사용할 수 있다. Q 객체 (django.db.models.Q)는 키워드 인수 모음을 캡슐화하는 데 사용되는 객체이다. 이러한 키워드 인수는 위의 "필드 조회"에서처럼 지정된다. 예를 들어 이 Q 객체는 단일 LIKE 쿼리를 캡슐화한다.

```python
from django.db.models import Q
Q(question__startswith='What')
```
Q 오브젝트는 &와 | 연산자를 사용하여 결합 될 수 있다. 연산자가 두 개의 Q 객체에 사용되면 새로운 Q 객체가 생성된다. 예를 들어, 이 구문은 두 개의 "question__startswith"쿼리의 "OR"을 나타내는 단일 Q 개체를 생성한다.

```python
Q(question__startswith='Who') | Q(question__startswith='What')
```
이것은 다음 SQL WHERE 절과 동일하다.

```sql
WHERE question LIKE 'Who%' OR question LIKE 'What%'
```

Q 오브젝트를 &와 |로 결합하여 임의의 복잡성을 갖는 문장을 작성할 수 있다. 연산자 및 괄호 그룹화를 사용g한다. 또한 Q 객체는 ~ 연산자를 사용하여 무효화 될 수 있으므로 일반 쿼리와 부정 (NOT) 쿼리를 결합한 결합 된 조회가 가능하다.

```python
Q(question__startswith='Who') | ~Q(pub_date__year=2005)
```
키워드 인수 (예 : filter (), exclude (), get ())를 사용하는 각 조회 함수는 하나 이상의 Q 객체를 위치 지정되지 않은 인수로 전달할 수도 있다. 조회 함수에 여러 개의 Q 오브젝트 인수를 제공하면 인수가 "AND"된다. 예를 들어,

```python
Poll.objects.get(
    Q(question__startswith='Who'),
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```

대략 SQL로 이렇게 해석된다.

```sql
SELECT * from polls WHERE question LIKE 'Who%'
    AND (pub_date = '2005-05-02' OR pub_date = '2005-05-06')
```

조회 함수는 Q 오브젝트와 키워드 인수의 사용을 혼합 할 수 있다. 조회 함수에 제공된 모든 인수 (키워드 인수 또는 Q 오브젝트 임)는 함께 AND된다. 그러나 Q 오브젝트가 제공되면, 키워드 인수의 정의 앞에 와야한다. 예 :

```python
Poll.objects.get(
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
    question__startswith='Who',
)
```

...는 앞의 예제와 같은 유효한 쿼리이다. 그러나

```python
# INVALID QUERY
Poll.objects.get(
    question__startswith='Who',
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```
는 유효하지 않을 것이다.

## Comparing objects
두 개의 모델 인스턴스를 비교하려면 표준 파이썬 비교 연산자 인 double = sign (==)을 사용하면 된다.

```python
>>> some_entry == other_entry
>>> some_entry.id == other_entry.id
```

모델의 기본 키가 id가 아닌 경우 문제가 없다. 비교에서는 항상 기본 키를 사용한다. 예를 들어, 모델의 기본 키 필드의 이름이 name 인 경우이 두 문은 동일합니다.

```python
>>> some_obj == other_obj
>>> some_obj.name == other_obj.name
```

##Deleting objects
delete 메서드는 편리하게 delete ()라는 이름을 갖는다. 이 메서드는 즉시 객체를 삭제하고 삭제 된 객체의 수와 객체 유형 당 삭제된 객체의 수를 담고 있는 딕셔너리를 반환한다. 예

```python
>>> e.delete()
(1, {'weblog.Entry': 1})
```

개체를 대량으로 삭제할 수도 있다. 모든 QuerySet에는 해당 QuerySet의 모든 멤버를 삭제하는 delete () 메소드가 있다. 다음 예는 pub_date 연도가 2005 인 모든 Entry 객체를 삭제한다.

```python
>>> Entry.objects.filter(pub_date__year=2005).delete()
(5, {'webapp.Entry': 5})
```

가능한 이것은 SQL로만 실행되므로 개개의 객체 인스턴스의 delete () 메소드는 반드시 프로세스 중에 호출되지는 않는다. 모델 클래스에 커스텀 delete () 메소드를 제공하고 그것이 호출되도록 하려면, 해당 모델의 인스턴스를 "수동으로"삭제해야 한다 (예 : QuerySet을 반복하고 delete ()를 호출하여). 각 개체를 개별적으로) 쿼리 집합의 대량 delete () 메서드를 사용하는 대신. Django는 객체를 삭제할 때 기본적으로 ON DELETE CASCADE SQL 동작의 동작을 에뮬레이션한다. 즉, 삭제 될 객체를 가리키는 외래 키가있는 객체는 함께 삭제된다. 예 :

```python
b = Blog.objects.get(pk=1)
# This will delete the Blog and all of its Entry objects.
b.delete()
```

이 계단식 동작은 ForeignKey에 대한 on_delete 인수를 통해 사용자 정의 할 수 있다. delete ()는 Manager 자체에 표시되지 않는 유일한 QuerySet 메서드이다. 이것은 실수로 Entry.objects.delete ()를 요청하지 않고 모든 항목을 삭제하지 못하게하는 안전 메커니즘이다. 모든 개체를 삭제하려면 명시적으로 전체 쿼리 집합을 요청해야 한다.

```python
Entry.objects.all().delete()
```
## Copying model instances
모델 인스턴스를 복사하기 위해 제공되는 기본적인 방법은 없지만 모든 필드의 값을 복사하여 새 인스턴스를 쉽게 만들 수 있다. 가장 단순한 경우 pk를 없음으로 설정할 수 있다. 블로그 예제 사용 :

```python
blog = Blog(name='My blog', tagline='Blogging is easy')
blog.save() # blog.pk == 1

blog.pk = None
blog.save() # blog.pk == 2
```
상속을 사용하면 상황이 더욱 복잡해진다. 블로그의 하위 클래스를 생각해보자.

```python
class ThemeBlog(Blog):
    theme = models.CharField(max_length=200)

django_blog = ThemeBlog(name='Django', tagline='Django is easy', theme='python')
django_blog.save() # django_blog.pk == 3
```

상속이 어떻게 작동하기 때문에 pk와 id를 None으로 설정해야 한다.

```python
django_blog.pk = None
django_blog.id = None
django_blog.save() # django_blog.pk == 4
```
이 프로세스는 모델의 데이터베이스 테이블에 포함되지 않은 관계는 복사하지 않는다. 예를 들어, 항목에는 작성자에 대한 ManyToManyField가 있다. 엔트리를 복제 한 후에는 새로운 엔트리에 대해 다 대다 관계를 설정해야 한다.

```python
entry = Entry.objects.all()[0] # some previous entry
old_authors = entry.authors.all()
entry.pk = None
entry.save()
entry.authors.set(old_authors)
```

OneToOneField의 경우 일대일 고유 제한을 위반하지 않도록 관련 개체를 복제하고 이를 새 개체의 필드에 할당해야 한다. 예를 들어, 항목이 이미 위와 같이 복제되었다고 가정하자.

```python
detail = EntryDetail.objects.all()[0]
detail.pk = None
detail.entry = entry
detail.save()
```
## Updating multiple objects at once
때로는 QuerySet의 모든 객체에 대해 특정 값으로 필드를 설정할 때가 있다. update () 메서드를 사용하여이 작업을 수행 할 수 있다. 예 :

```python
# Update all the headlines with pub_date in 2007.
Entry.objects.filter(pub_date__year=2007).update(headline='Everything is the same')
```

이 방법을 사용하여 비 관계 필드 및 ForeignKey 필드를 설정할 수 있다. 비 관계 필드를 업데이트하려면 새 값을 상수로 제공하면 된다. ForeignKey 필드를 업데이트하려면 새 값을 가리킬 새 모델 인스턴스로 설정하면 된다. 예 :

```python
>>> b = Blog.objects.get(pk=1)

# Change every Entry so that it belongs to this Blog.
>>> Entry.objects.all().update(blog=b)
```
update () 메서드는 즉시 적용되며 쿼리와 일치하는 행 수를 반환한다. 일부 행에 이미 새 값이있는 경우 업데이트 된 행 수와 다를 수 있다. 모델의 주 테이블 인 하나의 데이터베이스 테이블에만 액세스 할 수 있다는 점이 업데이트되는 QuerySet에 대한 유일한 제한이라고 할 수 있다. 관련 필드를 기반으로 필터링 할 수 있지만 모델 주 테이블의 열만 업데이트 할 수 있다. 예:

```python
>>> b = Blog.objects.get(pk=1)

# Update all the headlines belonging to this Blog.
>>> Entry.objects.select_related().filter(blog=b).update(headline='Everything is the same')
```

update () 메서드는 SQL 문으로 직접 변환된다. 직접 업데이트를 위한 대규모 작업이다. 이것은 모델에서 save () 메소드를 실행하지 않거나 save ()를 호출 한 결과인 pre_save 또는 post_save 신호를 내 보내지 않거나 auto_now 필드 옵션을 사용하지 않는다. QuerySet의 모든 항목을 저장하고 각 인스턴스에서 save () 메서드가 호출되도록 하려면 해당 함수를 처리하는 데 특별한 함수가 필요하지 않다. 그냥 루프를 돌리고 save ()를 호출하면 된다.

```python
for item in my_queryset:
    item.save()
```
업데이트 호출은 F 식을 사용하여 모델의 다른 필드 값을 기반으로 한 필드를 업데이트 할 수도 있다. 이것은 현재 값에 따라 수를 증가시킬 때 특히 유용다. 예를 들어 블로그의 모든 항목에 대한 핑백 수를 늘리려면 다음과 같이하면 된다.

```python
>>> Entry.objects.all().update(n_pingbacks=F('n_pingbacks') + 1)
```

그러나 필터 및 제외 절의 F () 개체와 달리 업데이트에서 F () 개체를 사용할 때 결합 구문을 도입 할 수는 없으며 업데이트되는 모델의 로컬 필드만 참조 할 수 있다. F () 객체를 사용하여 결합 구문을 도입하려고 시도하면 FieldError가 발생한다.

```python
# This will raise a FieldError
>>> Entry.objects.update(headline=F('blog__name'))
```

## Related objects
모델 (즉, ForeignKey, OneToOneField 또는 ManyToManyField)에서 관계를 정의하면 해당 모델의 인스턴스는 관련 객체에 액세스하기위한 편리한 API를 갖게 된다. 예를 들어, 이 페이지 상단의 모델을 사용하여 Entry 객체 e는 블로그 속성인 e.blog에 액세스하여 관련 Blog 객체를 가져올 수 있다. 배후에서 이 기능은 파이썬 설명자에 의해 구현된다. Django는 관계의 "다른"쪽에 대한 API 접근 자 - 관련 모델에서 관계를 정의하는 모델에 대한 링크 -를 만든다. 예를 들어, Blog 객체 b는 entry_set 속성인 b.entry_set.all ()을 통해 관련된 모든 Entry 객체의 목록에 액세스 할 수 있다. 이 섹션의 모든 예제는 이 페이지의 맨 위에 정의 된 샘플 Blog, Author 및 Entry 모델을 사용한다.

### One-to-many relationships
모델에 ForeignKey가있는 경우 해당 모델의 인스턴스는 모델의 단순 속성을 통해 관련 (외부) 객체에 액세스 할 수 있다. 예:

```python
>>> e = Entry.objects.get(id=2)
>>> e.blog # Returns the related Blog object.
```

외부 키 속성을 통해 가져오고 설정할 수 있다. 예상대로 외래 키에 대한 변경 사항은 save ()를 호출 할 때까지 데이터베이스에 저장되지 않는다. 예:

```python
>>> e = Entry.objects.get(id=2)
>>> e.blog = some_blog
>>> e.save()
```

ForeignKey 필드에 null = True가 설정되어 있으면 (즉, NULL 값을 허용하는 경우) None을 할당하여 관계를 제거 할 수 있다. 예:

```python
>>> e = Entry.objects.get(id=2)
>>> e.blog = None
>>> e.save() # "UPDATE blog_entry SET blog_id = NULL ...;"
```

관련 객체에 처음 액세스 할 때 일대 다 관계에 대한 전달 액세스가 캐시된다. 동일한 오브젝트 인스턴스에서 외래 키에 대한 이어지는 액세스들이 캐시된다.

```python
>>> e = Entry.objects.get(id=2)
>>> print(e.blog)  # Hits the database to retrieve the associated Blog.
>>> print(e.blog)  # Doesn't hit the database; uses cached version.
```

select_related () QuerySet 메서드는 모든 일대 다 관계의 캐시를 미리 재귀적으로 미리 채운다.

```python
>>> e = Entry.objects.select_related().get(id=2)
>>> print(e.blog)  # Doesn't hit the database; uses cached version.
>>> print(e.blog)  # Doesn't hit the database; uses cached version.
```

#### Following relationships “backward”
모델에 ForeignKey가있는 경우 외래 키 모델의 인스턴스는 첫 번째 모델의 모든 인스턴스를 반환하는 관리자에 액세스 할 수 있다. 기본적으로이 Manager의 이름은 FOO_set이다. 여기서 FOO는 소문자 인 소스 모델 이름이다. 이 Manager는 위의 "개체 검색"절에서 설명한대로 필터링 및 조작 할 수있는 QuerySet을 반환한다.

```python
>>> b = Blog.objects.get(id=1)
>>> b.entry_set.all() # Returns all Entry objects related to Blog.

# b.entry_set is a Manager that returns QuerySets.
>>> b.entry_set.filter(headline__contains='Lennon')
>>> b.entry_set.count()
```

ForeignKey 정의에서 related_name 매개 변수를 설정하여 FOO_set 이름을 겹쳐 쓸 수 있다. 예를 들어 항목 모델이 blog = ForeignKey (블로그, on_delete = models.CASCADE, related_name = 'entries')로 변경된 경우 위의 예제 코드는 다음과 같다.

```python
>>> b = Blog.objects.get(id=1)
>>> b.entries.all() # Returns all Entry objects related to Blog.

# b.entries is a Manager that returns QuerySets.
>>> b.entries.filter(headline__contains='Lennon')
>>> b.entries.count()
```

#### Using a custom reverse manager
기본적으로 역 관계에 사용되는 RelatedManager는 해당 모델의 기본 관리자의 하위 클래스이다. 특정 쿼리에 대해 다른 관리자를 지정하려면 다음 구문을 사용할 수 있다.

```python
from django.db import models

class Entry(models.Model):
    #...
    objects = models.Manager()  # Default Manager
    entries = EntryManager()    # Custom Manager

b = Blog.objects.get(id=1)
b.entry_set(manager='entries').all()
```

EntryManager가 get_queryset () 메소드에서 기본 필터링을 수행하면, 해당 필터링이 all () 호출에 적용된다. 물론 사용자 지정 역방향 관리자를 지정하면 사용자 지정 메서드를 호출 할 수도 있다.

```python
b.entry_set(manager='entries').is_published()
```

ForeignKey Manager에는, 관련 지을 수 있었던 오브젝트의 집합을 처리하기 위해서 사용되는 메소드가 있다. 각각의 개요가 아래에 있으며, 자세한 내용은 관련 객체 참조에서 찾을 수 있다.

- add (obj1, obj2, ...) 지정된 모델 객체를 관련 객체 세트에 추가한다.
- create (** kwargs) 새 객체를 만들고 저장 한 다음 관련 객체 세트에 배치한다. 새롭게 생성 된 객체를 돌려준다.
- remove (obj1, obj2, ...) 관련 객체 세트에서 지정된 모델 객체를 제거한다.
- clear () 관련 객체 세트에서 모든 객체를 제거한다.
- set (objs) 관련 오브젝트 세트를 대체한다.

관련 세트의 멤버를 할당하려면 객체 인스턴스의 반복 가능 또는 기본 키 값 목록과 함께 set () 메소드를 사용하면 된다. 예 :

```python
b = Blog.objects.get(id=1)
b.entry_set.set([e1, e2])
```
이 예에서 e1 및 e2는 전체 Entry 인스턴스 또는 정수 기본 키 값이 될 수 있다. clear () 메서드를 사용할 수 있는 경우, iterable (이 경우 목록)의 모든 객체가 세트에 추가되기 전에 기존 객체가 entry_set에서 제거된다. clear () 메서드를 사용할 수 없는 경우, 반복 가능한 모든 요소는 기존 요소를 제거하지 않고 추가된다. 이 섹션에서 설명한 "역방향"작업은 데이터베이스에 즉각적인 영향을 준다. 추가, 생성 및 삭제가 즉시 자동으로 데이터베이스에 저장된다.

### Many-to-many relationships
다 - 대 - 다 관계의 양 끝은 상대방에 대한 자동 API 액세스를 얻는다. API는 위의 "역방향" 일대 다 관계와 동일하게 작동한다. 유일한 차이는 속성 이름 지정에 있습니다. ManyToManyField를 정의하는 모델은 해당 필드 자체의 속성 이름을 사용하지만 "역방향"모델은 원본 모델의 소문자 모델 이름과 '_set'으로 사용한다.

```python
e = Entry.objects.get(id=3)
e.authors.all() # Returns all Author objects for this Entry.
e.authors.count()
e.authors.filter(name__contains='John')

a = Author.objects.get(id=5)
a.entry_set.all() # Returns all Entry objects for this Author.
```
ForeignKey와 마찬가지로 ManyToManyField는 related_name을 지정할 수 있다. 위의 예에서 Entry의 ManyToManyField가 related_name = 'entries'를 지정한 경우, 각 Author 인스턴스는 entry_set 대신 entries 속성을 갖는다.

### One-to-one relationships
일대일 관계는 다 대일 관계와 매우 유사하다. 모델에 OneToOneField를 정의하면 해당 모델의 인스턴스는 모델의 단순 속성을 통해 관련 객체에 액세스 할 수 있다.

```python
class EntryDetail(models.Model):
    entry = models.OneToOneField(Entry, on_delete=models.CASCADE)
    details = models.TextField()

ed = EntryDetail.objects.get(id=2)
ed.entry # Returns the related Entry object.
```
차이점은 "역"쿼리이다. 1 대 1 관계의 관련 모델도 Manager 객체에 액세스 할 수 있지만, Manager는 객체 컬렉션이 아닌 단일 객체를 나타낸다.

```python
e = Entry.objects.get(id=2)
e.entrydetail # returns the related EntryDetail object
```

이 관계에 객체가 할당되어 있지 않으면 Django는 DoesNotExist 예외를 발생시킨다. 전달 관계를 지정하는 것과 같은 방법으로 인스턴스를 역 관계에 지정할 수 있다.

```python
e.entrydetail = ed
```

### 역참조가 가능한 이유

다른 객체 관계형 맵퍼에서는 양측에 관계를 정의해야 한다. Django 개발자는 DRY (Do not Repeat Yourself) 원칙을 위반하므로 Django는 한쪽 끝에서만 관계를 정의해야 한다고 생각한다. 그러나 모델 클래스가 다른 모델 클래스가로드 될 때까지 어떤 다른 모델 클래스가 관련되어 있는지를 모델 클래스가 알지 못한다면 어떻게 가능할까? 대답은 앱 레지스트리에 있다. Django가 시작되면 INSTALLED_APPS에 나열된 각 응용 프로그램을 가져온 다음 각 응용 프로그램 내부에 models 모듈을 가져온다. 새로운 모델 클래스가 생성 될 때마다 Django는 모든 관련 모델에 역방향 관계를 추가한다. 관련 모델을 아직 가져 오지 않은 경우, 장고는 관련 모델을 가져올 때 관계의 트랙을 유지하고 추가한다. 이러한 이유로 INSTALLED_APPS에 나열된 응용 프로그램에서 사용중인 모든 모델을 정의하는 것이 특히 중요하다. 그렇지 않으면 후방 관계가 제대로 작동하지 않을 수 있습니다.

```pythn
Entry.objects.filter(blog=b) # Query using object instance
Entry.objects.filter(blog=b.id) # Query using id from instance
Entry.objects.filter(blog=5) # Query using id directly
```
b의 id값이 5인 경우 위의 셋은 동일하다.




