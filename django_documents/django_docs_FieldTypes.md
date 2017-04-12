# Model field reference



django.db.models(django.db.models.fields)

from django.db import models

models.<Foo>Field

## Field options
다음 arguments는 모든 field type에서 사용가능하다. 모두 선택적으로 사용가능하다.

**Field.**

### null

### blank

### choices

### db_column
필드에 사용할 데이터베이스 컬럼의 이름. 이 인자가 주어지지 않으면 장고는 필드의 이름을 사용한다. 

### db_index
True이면, 데이터베이스 인덱스가 생성된다. 

### db_tablespace
인덱스를 사용하기 위해 데이터베이스 테이블 공간명을 사용한다. 백엔드가 인덱스를 위한 테이블공간을 지원하지 않으면, 이 옵션은 무시된다. 

### default
필드의 기본값 설정

### editable
False면 필드가 표시되지 않는다. 모델 유효성 검사를 건너뛴다. 기본값은 True

### error_messages
딕셔너리 형태로 원하는 에러메세지를 오버라이드 해서 사용할 수 있다. error_message 키는 다음과 같다. null, blank, invalid, invalid_choice, unique, and unique_for_date.

### help_text
폼 위젯에 도움말 텍스트를 추가해준다.

### primary_key

### unique

### unique_for_date
DateField나 DateTimeField의 이름으로 설정하면 DateField의 유니크한 값이 된다??

### unique_for_month
### unique_for_year
### verbose_name
가독성 좋은 이름을 필드에 지정한다. 지정해주지 않으면 장고가 자동으로 필드속성으로 필드이름을 지정한다. 공백은 언더바 처리된다.

### validators
현재 필드에 대해 실행되는 유효성 검사 목록이다.

#### Registering and fetching lookups

## [Field types](https://docs.djangoproject.com/en/1.10/ref/models/fields/#field-types)

**class**

### AutoField
IntegerField로 자동으로 ID를 생성한다. 

### BigAutoField
64bit IntegerField. 1부터


### BigIntegerField
64bit. -부터. 기본 폼 양식은 TextInput

>왜 textinput????

### BinaryField
이진데이터를 저장하는 필드. bytes만 지원한다. 쿼리셋 필터링을 못 한다. ModelForm에도 이 필드를 포함할 수 없다. 이 필드에 파일을 담는 것은 안좋다!

### BooleanField
기본 폼 양식은 CheckboxInput. Null을 허용하고 싶으면 NullBooleanField를 사용해야 한다. default옵션이 설정되어 있지 않으면 기본값은 None이다.

### CharField
문자열 필드. 많은양의 텍스트를 담고싶으면 TextField를 사용해야 한다. 기본 폼 양식은 TextInput. max_length옵션을 지정해줘야 한다. 

### CommaSeparatedIntegerField
`,`로 구분된 IntegerField. max_length를 지정해주어야 한다.

>주의! 각 데이터베이스마다 제한하는 max_length의 크기에 주의해야 한다.

### DateField
날짜는 파이썬에서 datetime.date인스턴스로 표현된다. 추가의 선택적인 arguments가 있다.

* DateField.auto_now
	객체가 저장될 때 마다 지금 시간을 자동으로 저장한다. 오버라이드 할 수 있는 기본값이 아니다. Model.save()할 때마다 자동으로 업데이트 된다. ????
	
* DateField.auto_now_add
	객체가 처음 생성될 때 자동으로 날짜를 설정한다. 타임스탬프 생성에 유용하다. 오버라이드 할 수 없기때문에 객체가 생성될 때 값을 설정하고 싶어도 무시된다. 이 필드에 값을 다르게 주려면 옵션값을 True로 주는 대신, 다음과 같이 해야 한다. 
	* For DateField: default=date.today - from datetime.date.today()
	* For DateTimeField: default=timezone.now - from django.utils.timezone.now()

	기본 폼 양식은 TextInput. 
	
> auto_now와 auto_now_add를 True로 하면, editable=False와 blank=True도 발생하게 한다.



### DateTimeField

### DecimalField
고정소수점 필드. 파이썬의 Decimal 인스턴스로 표현된다. 다음 두가지 옵션이 요구된다.

* DecimalField.max_digits
	숫자에 허용되는 최대 자릿수. 소수자릿수보다 크거나 같아야 한다.
* DecimalField.decimal_places
	소수자릿수
	
기본 폼 형식은 NumberInput과 localize=False 혹은 TextInput이다.

### DurationField
파이썬에서 timedelta로 모델링된 시간데이터를 저장하는 필드. PostgreSQL을 제외한 데이터베이스들에서 DurationField와 DateTimeField의 계산결과는 비교하면 같지않다.

### EmailField
이메일 주소가 유효한지 확인하는 CharField. EmailValidator를 사용해 유효성검사 한다.

### FileField
파일 업로드 필드.
primary_key와 uinque가 지원되지 않으며 TypeError를 발생시킬 수 있다. 

#### FileField.upload_to
업로드 디렉토리와 파일명을 설정할 수 있다. 설정된 값은 Storage.save()로 저장된다. strftime()포맷의 스트링값을 입력한 부분은 경로명으로 대체되지 않고 업로드 시간으로 대체된다. 기본 FileSystemStorage를 이용하면 로컬의 MEDIA_ROOT 경로에 저장된다. `/media/app_name/file_name.*`

upload_to는 호출가능하다. 다음의 두개의 인자와 함께 호출되어 파일 업로드 경로를 반환한다.

* instance : FileField로 정의된 인스턴스. 현재 파일이 첨부된 경로? 대부분 아직 데이터베이스에 저장되지 않아서 primary key 필드가 생성되지 않은 상태다. 상대경로. 

* filename : 파일에 원래 주어진 이름. 최종 업로드 경로를 결정할때 주어지기도 하고 아닐 수도 있다.

다음의 예 참고,
```python
def user_directory_path(instance, filename):
    # file will be uploaded to MEDIA_ROOT/user_<id>/<filename>
    return 'user_{0}/{1}'.format(instance.user.id, filename)

class MyModel(models.Model):
    upload = models.FileField(upload_to=user_directory_path)
```

#### FileField.storage
파일의 저장 및 검색을 처리하는 저장객체. 기본 폼 위젯은 ClearableFileInput.

모델에서 FileField와 ImageField를 사용하는 단계는 다음과 같다.

1. settings.py에서 MEDIA_ROOT에 장고가 업로드된 파일을 저장할 디렉토리 full path를 정의해준다. (성능을 위해 데이터베이스에 저장되지 않는다.) 이 디렉토리의 기본 공개 URL을 MEDIA_URL에 정의해준다. 웹서버의 사용자 계정에서 이 디렉토리에 접근 가능한지 확인해라.
2. FileField나 ImageField를 모델에 추가한다. upload_to옵션에 업로드된 파일을 저장할 MEDIA_ROOT의 하위 디렉토리를 추가해준다.
3. 이렇게 데이터베이스에 저장될 경로들은 업로드된 파일의 경로이다. (MEDIA_ROOT에 대한 상대경로) 장고에서 제공되는 편리한 url속성을 사용하고 싶을것이다. 예를들어 mug_shot이라는 변수에 할당된 ImageField가 있을때, image파일의 절대경로를 얻을 수 있는 {{ object.mug_shot.url }}템플릿 코드 같은것. 

예를들어, MEDIA_ROOT가 `/home/media`이고 upload_to는 `photos/%Y/%m/%d`일때, 2017년 2월 11일에 파일을 업로드 한다고 하면 저장될 경로는 `/home/media/photos/2017/02/11`이 된다.

만약 물리적 디스크에 저장된 파일명이나 크기를 얻고 싶다면, name이나 size속성을 사용할 수 있다. 자세한것은 [File객체](https://docs.djangoproject.com/en/1.10/ref/files/file/#django.core.files.File) 혹은 [파일다루기](https://docs.djangoproject.com/en/1.10/topics/files/) 참고.

업로드된 파일의 상대URL은 url속성으로 얻을 수 있다. 이것은 Storage.url()을 호출한다. 업로드 파일을 다룰때 보안에 주의를 기울여야 한다. 웹 서버에 누군가 스크립트를 업로드 해서 실행시킬 수 도 있다!!

FileField 인스턴스는 데이터베이스 varchar컬럼으로 max length 100자의 크기로 생성된다. 

### FileField and FieldFile
모델의 FileField에 접근하면 기본 파일에 접근하기 위한 프록시로 FieldFile인스턴스가 제공된다. FieldFile API는 File

### FilePathField

### FloatField
float인스턴스에 의해 표현된 부동 소수점 숫자 필드. 기본 폼 위젯은 NumberInput에 localize False이거나 TextInput.

### ImageField

### IntegerField

### GenericIPAddressField

### NullBooleanField

### PositiveIntegerField

### PositiveSmallIntegerField

### SlugField

### SmallIntegerField

### TextField

### TimeField

### URLField

### UUIDField

## Relationship fields
장고는 관계를 나타내는 필드를 정의한다.

### ForeignKey
다대일 관계. 위치 인수(모델이 연관된 클래스)가 요구된다. 

>on_delete 이제 위치인수로 사용할 수 있다. (이전 버전의 장고에서는 키워드 인수로만 쓰였다.) 장고2.0에서는 필수인수다.



select related , prefetched
포린키에 연결된 데이터 미리 가져와서 캐시해둔다 all과는 다름 

피클링,
unique를 왜 매니투매니랑 원투원에서 못쓰나

### SlugField
모델을 url로 표현할때 