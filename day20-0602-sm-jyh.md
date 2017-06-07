# Models
- 모델이란 장고 내에서 데이터베이스의 구조(테이블과 필드)를 관리할 수 있는 모듈이다.
- 모델의 각 속성은 데이터베이스의 필드를 나타낸다.
- Django는 모델을 통해 자동으로 생성된 데이터 베이스에 액세스 할 수 있는 API를 제공한다.

__기본 구조__

	from django.db import models

	class Person(models.Model):
		first_name = models.CharField(max_length=30)
		last_name = models.CharField(max_length=30)
	
	#이 코드는 아래의 쿼리문과 동일하다.
	
	CREATE TABLE myapp_person (
		"id" serial NOT NULL PRIMARY KEY,
		"first_name" varchar(30) NOT NULL,
		"last_name" varchar(30) NOT NULL
	);
	
장고 모델을 사용하기 위해선 settings.py 파일 내INSTALLED_APP에 해당 model이 있는 app을 추가한 뒤 마이그레이션 파일을 만들어줘야 한다.

## Field
필드를 정의할 때 여러가지 속성을 줄 수 있다. (쿼리문과 대부분 비슷하다.) 필드는 클래스의 속성에 의해 지정된다.

### Field types
모델의 각 필드는 해당 클래스의 인스턴스이어야 한다.
Django는 필드 클래스 유형을 사용해 몇 가지를 결정한다.
- DB에 저장할 데이터의 종류(ex: INTEGER, VARCHAR, TEXT)
- Form field를 렌더링 할 때 사용할 HTML 위젯 (ex: `<input type='text'>`, `<select>` )

### Field options
각 필드는 특정한 arguments sets을 갖는다. 예를 들어, `CharField`는 DB의 VARCHAR 필드에 저장되는 데이터의 촤대 길이를 제한하는 `max_length` argument를 필요로 한다.

* null/blank
	* null 혹은 blank를 허용할지에 대한 여부다. 다만 null과 blank는 다르다는 점을 명심하자.
* choices
	* 초이스 항목에 튜플들이 담긴 리스트를 전달하면, 선택 가능한 항목이 된다.
* default
	* 기본 값을 정할 수 있음.
* help_text
	* 도움 텍스트를 작성하게 해줌.
* primary_key
* unique

__필드 명__
필드 명은 지정하지 않았을 경우 자동으로 변수명으로 지정해주지만, 원하는 값으로 지정할 수 있다. 그 땐 `verbose_name` 값에 문자열을 할당하면 된다.

## 관계형 데이터베이스
데이터베이스는 서로 관계를 가질 수 있다.

### Many to one
다대일 관계를 위해선 ForeignKey를 사용하면 되며, 평범하게 필드를 선언하듯이 선언하면 되고, 패러미터로 모델 클래스 혹은 문자열로 지정하면 된다.

> 다대일 관계란 Foreignkey(클래스 C)를 사용한 테이블에서는 단 한개의 C만을 가질 수 있지만, 클래스 C는 다수의 해당 테이블을 가질 수 있기 때문이다.

### Many to many
다대다 관계를 위해서는 ManyToManyField를 사용하면 된다. 역시나 일반 필드 생성하듯 생성하면 된다. 다대일 관계처럼 모델 클래스를 받아들일 수 있으며, 다수의 해당 모델을 가질 수 있다. 이를테면 피자의 토핑을 설정하기 위해 다대다 필드로 토핑을 추가하면 되는 것처럼.

하지만 가끔 중간 과정이 필요한 복잡한 다대다 관계를 지정해야 할 때가 있다. 그럴 때엔 `through` 인자를 사용하면 된다.

	class Person(models.Model):
		name = models.CharField(max_length=128)

		def __str__(self):              # __unicode__ on Python 2
		    return self.name

	class Group(models.Model):
		name = models.CharField(max_length=128)
		members = models.ManyToManyField(Person, through='Membership')

		def __str__(self):              # __unicode__ on Python 2
		    return self.name

	class Membership(models.Model):
		person = models.ForeignKey(Person, on_delete=models.CASCADE)
		group = models.ForeignKey(Group, on_delete=models.CASCADE)
		date_joined = models.DateField()
		invite_reason = models.CharField(max_length=64)
		
다음과 같은 구조에서는 각 사람들의 그룹을 추적할 수 있게끔 해야 하지만, 그 뿐만 아니라 그룹에 참가한 날짜, 초대받은 이유 등까지도 접근해야 한다. 그럴 때라면 그룹을 중개 모델로써 활용하면 되며, 중개 모델은 왜래 키를 명시적으로 지정한다.

> 만일 `through`가 없이 foreign key가 여러개라면 오류를 출력한다. 외래 키가 늘어나면 그에 맞춰서 `thourgh`역시 추가해야한다.

### One to one
일대일 관계는 예상했다 시피 `OneToOneField`를 사용하면 된다.

만일 장소라는 모델을 설정하고, 그 장소의 표준적 정보들(번호, 주소 등등)을 추가한 뒤에 레스토랑이라는 모델을 만들려 한다면, 굳이 데이터베이스를 복제할 필요 없이 일대일 대응으로 받아오면 된다. 사실 이럴 때엔 일대일 관계를 포함하고 있는 '상속'을 사용하면 되기도 하다.

__다른 앱 내의 모델__
다른 앱 내의 모델을 가져올 땐 그냥 해당 파일을 임포트하면 된다.

__이름 제한__
장고의 모델의 필드 이름엔 오직 두가지 제약만 존재한다. 예약어와 더블 언더바.
> sql 예약어를 사용하더라도 상관없다. 장고는 모든 필드 명을 sql 상에서 탈출하여 사용하기 때문.

__커스텀 필드__
[문서 참조](https://docs.djangoproject.com/en/1.11/howto/custom-model-fields/) 

### 메타 옵션
메타데이터 속성을 주기 위해선 내부 클래스로 `Meta`를 정의하면 된다.

	class Ox(models.Model):
		horn_length = models.IntegerField()

	class Meta:
		ordering = ["horn_length"]
		verbose_name_plural = "oxen"

메타 데이터는 필드가 아닌 모든 것들이 들어간다. 이를 태면 정렬 방식, 테이블 이름 등이 있을 것이다.

### 모델 매니져
모델 매니져는 해당 모델에 접근하기 위한 것으로 특별히 정의하지 않으면 기본적으로는 `objects`로 사용할 수 있다.

### 오버라이딩
데이터베이스의 관리를 위한 메서드들을 오버라이딩을 통해 커스터마이징 할 수 있다. 그 중에서 세이브와 딜리트는 자주 사용할 것이다. 세이브에 조건을 추가하여 부합하지 않으면 세이브를 거부할 수도 있고, 뭔가 추가적인 정보를 추가할 수도 있으며, 딜리트 역시 그러하게 할 수 있다.

> 명심할 부분은 항상 수퍼 클래스의 메서드를 재 호출 해야 한다는 점이다. (그 기능을 완전히 바꾸지 않는 한은.)

### 모델 상속
파이썬의 기본적인 상속처럼 모델 역시 상속 기능이 가능하지만, 기본적으론 장고 데이터베이스 모델의 틀을 따라야 한다. 고려해야 할 상황은 오직 부모 모델이 자신만의 모델이 될 지, 혹은 부모가 자식 모델을 통해서만 볼 수 있는 정보를 보유하는지 뿐이다.

다음은 상속의 세가지 방법이다.
* 부모 클래스에 미리 정보를 기록해둠으로 자식 클래스에 기록하지 않더라도 자동으로 데이터를 갖게끔 하고자 한다면 기본 추상 클래스를 사용하면 된다.
* 기존 모델을 하위 클래스화 하고 각 모델에 고유 데이터베이스 테이블을 갖게 하고자 한다면 다중 테이블 상속이 필요하다.
* 모델 필드의 변경 없이 파이썬 수준 동작만 수정하려는 경우 프록시 모델을 사용하면 된다.

### 추상 클래스
추상 클래스는 다수의 클래스가 공통적인 데이터를 가질 경우 그것을 묶는데 유용하다. 내부 클래스 메타에 `abstract` 속성을 `True`로 할당한다면 해당 클래스를 추상화가 가능하며, 해당 클래스는 데이터베이스가 형성되지 않는다.

> 추상 클래스를 상속받아도 해당 클래스의 기본 추상 속성값은 `False`다.

### 다중 테이블 상속
만일 상속 계층의 모델이 모두 하나의 모델일 때, 즉 각 모델이 하나의 데이터베이스 테이블에 해당할 때 사용할 수 있으며, 자동으로 생성된 일대일 필드에 링크를 함으로 가능하다.

	models.OneToOneField(
		Place, on_delete=models.CASCADE,
		parent_link=True,
	)
	
이럴 경우 상속받은 자식 클래스에서 부모 클래스로 접근이 가능해 진다.

### 프록시 상속
데이터베이스를 생성하지 않고, 단순한 파이썬 기능만을 사용한 상속을 원할 때엔 프록시 상속을 사용하면 된다. 메타 클래스에 프록시 속성값을 `True`로 설정하면 된다. 

	class Person(models.Model):
		first_name = models.CharField(max_length=30)
		last_name = models.CharField(max_length=30)

	class MyPerson(Person):
		class Meta:
		proxy = True
	
		def do_something(self):
			# ...
			pass
			
이 경우에는 마이펄슨은 데이터베이스가 존재하지 않지만, 메서드를 추가하여 펄슨에서 마이 펄슨의 메서드를 사용 가능하며, 마이펄슨에서 펄슨의 인스턴스에 접근하는 것 역시 가능하다.

> 프록시 모델은 반드시 추상 모델이 아닌 모델을 상속해야 한다.

프록시 모델에 관리자를 추가하지 않으면 부모의 관리자를 그대로 받아 사용한다.

### 다중 상속
다중 상속 시에는 파이썬의 기본 이름 해석 규칙이 적용된다. 만약 메타라는 것이 여러개 존재한다면 그 중에 첫번째로 상속받은 것을 적용시킨다.

일반적으로 다중 상속을 사용할 일은 거의 없지만, 믹스인 클래스를 사용할 때라면 필요해질 수도 있다. 믹스인은 상속받은 모든 클래스에 특정 메서드나 속성을 추가할 때 사용된다.

공통 ID 기본 키가 있는 필드를 상속할 경우 에러가 뜨니 다중 상속시에는 명시적 자동 채우기를 사용하면 좋다. `AutoField(primary_key=True)`가 그 기능을 한다.

> 필드의 이름 숨기기는 허용되지 않는다.
