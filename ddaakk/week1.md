## 1장) 루비 시작하기

레거시 프로젝트에서는 RVM(Ruby Version Manager)를 통해 Ruby 버전 관리 + GemSet 관리 기능을 사용했다.
모던 Ruby 개발에서는 bundler가 사실상 표준이 되었고, rbenv(루비 버전 관리) + bundler(GemSet 관리)를 통해 프로젝트를 관리한다.

소스 코드를 RDoc을 사용해서 문서화가 가능하다.

```ruby
class Calculator
  # 두 수를 더합니다.
  #
  # [a] 첫 번째 숫자
  # [b] 두 번째 숫자
  #
  # Returns:: 더한 결과
  #
  #   calc = Calculator.new
  #   calc.add(2, 3)  #=> 5

  def add(a, b)
    a + b
  end

  # 두 수를 곱합니다.
  #
  # === 예시:
  #   calc.multiply(4, 5)  #=> 20

  def multiply(a, b)
    a * b
  end

  # 나눗셈을 수행합니다.
  #
  # [dividend] 나누어질 수
  # [divisor] 나누는 수
  #
  # Returns:: 나눈 결과
  # Raises:: ZeroDivisionError 0으로 나누는 경우

  def divide(dividend, divisor)
    raise ZeroDivisionError if divisor.zero?
    dividend / divisor
  end
end
```

현존하는 모든 메서드와 클래스, 모듈에 이미 RDoc을 통해 내부적으로 문서화되어 있다.
RDoc으로 문서화된 경우 파일의 문서 부분을 추출하여 HTML과 ri 형식으로 변환할 수 있다.
ri는 같은 문서를 로컬에서 볼 수 있는 명령행 뷰어 프로그램이다.

```bash
$ ri GC # 클래스 요약
$ ri GC::enable # 클래스의 메서드 요약
$ ri assoc # 중복된 메서드가 여러 모듈에 존재할 경우 전부 출력
```

## 2장) Ruby.new

루비는 진정한 객체 지향 언어이다.
루비에서 프로그래머가 다루는 모든 것은 객체이고 이러한 작업의 결과 또한 객체이다.

다음은 조영호님의 객체지향의 사실과 오해 책에서 가져온 객체지향에 대한 개념이다.

---

### 1. 객체의 본질
객체지향의 목표는 실세계를 모방하는 것이 아니라 새로운 세계를 창조하는 것이다 (모델링).
객체지향이랑 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해 시스템을 분할하는 방법이다.

### 2. 핵심 트라이어드

#### 역할(Role)
- 역할은 어떤 협력에 참여하는 특정한 사람이 협력에서 차지하는 책임이나 임무이다
- 여러 객체가 동일한 역할을 수행할 수 있고, 역할은 대체 가능성을 의미한다
#### 책임(Responsibility)
- 적절한 책임은 객체지향 설계의 품질을 결정하는 가장 중요한 요소이다
- 객체가 협력에서 수행해야 하는 임무이다
#### 협력(Collaboration)
일반적으로 하나의 문제를 해결하기 위해 다수의 사람 혹은 역할이 필요하며, 요청이 또 다른 사람에 대한 요청을 유발한다

### 3. 메세지와 메서드

#### 메세지
- 객체지향의 세계에서 의사소통을 '메세지'라고 한다
- 객체지향의 핵심은 메세지이다
- 객체 간의 상호작용 방식을 의미한다
- 메세지 = 메서드 이름 + 파라미터

#### 메서드
- 객체가 수신된 메세지를 처리하는 방법을 메서드라고 한다
- 외부의 요청이 무엇인지를 표현하는 메세지와 요청을 처리하기 위한 구체적인 방법인 메서드를 분리하는 것은 객체의 자율성을 높이는 핵심 메커니즘이다

### 4. 상태와 행동

#### 상태(State)
- 바리스타가 커피를 제조하기 위해서는 어떤 제조법을 기억하고 있는지 스스로 판단하는 것처럼 객체는 스스로 자율성을 가지고 요청에 대해 행동하기 위한 상태를 가져야 한다

#### 행동과 상태의 관계
- 상태와 행동을 함께 가지고 있는 객체는 협력관계에서 자율적인 객체로 격상되며, 데이터와 프로세스를 엄격히 구분하는 전통적인 개발방법과 객체지향을 구분짓는 핵심적인 차이점이다
- 행동이 상태를 결정하도록 해야하고, 시스템을 메세지를 주고받는 동적인 객체들의 집합으로 바라봐야 한다

### 5. 자율성과 캡슐화

#### 자율성
- 자율적인 객체란 상태와 행위를 함께 지니며 스스로 자기 자신을 책임지는 객이다
- 각 객체는 책임을 수행하는 방법을 자율적으로 선택할 수 있다

#### 캡슐화
- 메세지와 메서드를 분리하는 것은 캡슐화라는 개념과도 깊이 관련있다
- 캡슐화를 통해 외부 요청과 이를 처리하는 내부 로직을 명확하게 분리해내는 것이 유연하고 재사용에 좋은 객체를 만들어내는 시작이다

### 6. 타입과 분류

#### 타입
- 타입은 개념과 동일하다. 따라서 타입이란 우리가 인식하고 있는 다양한 사물이나 객체에 적용할 수 있는 아이디어나 관념이다
- 객체가 어떤 행동을 하느냐에 따라 객체의 타입이 결정된다

---

### 루비 기초

자바와 달리 루비에는 숫자 객체가 절댓값을 구하는 기능을 가지고 있다.
우리는 abs 메세지를 숫자 객체에 보내서 이를 처리하도록 요청하면 된다.

```ruby
num = -1234        # => -1234
positive = num.abs # => 1234
```

이 방식은 모든 루비 객체에 적용된다.
C에서는 문자열 길이를 얻기 위해 strlen(name)이라고 작성해야 하지만, 루비에서는 name.length와 같이 사용한다.
이 점이 바로 루비가 진정한 객체 지향 언어라고 하는 말에 담긴 의미이다.

루비는 세미 콜론이 필요없고, 주석문은 # 문자로 시작해서 그 줄의 끝까지 적용된다.
인덴트도 파이썬처럼 필수적인 요소가 아니며 가독성을 위해 정렬하는 정도이다.
루비는 중괄호 대신 메서드 끝 부분에 end 키워드가 필요하다.

메서드 정의에는 def 키워드를 사용하며 def 다음에 메서드 이름을 쓰고 그 다음에는 괄호로 싸인 메서드 매개 변수를 쓴다(이때 괄호는 생략해도 상관없지만 있는 편을 선호한다)

```ruby
def say_goodnight(name)
  result = "Good night, " + name
  return result
end

def say_goodnight name
  result = "Good night, " + name
  return result
end
```

puts 함수는 매개 변수를 출력할 때 위치를 다음 줄로 옮겨주는 줄 바꿈을 붙여서 출력한다.

```ruby
puts say_goodnight("John-Boy")
puts(say_goodnight("John-Boy"))
```

루비에서는 다음 두 표현식이 뜻하는 바가 같다. 단지 취향의 문제이다.
하지만 우선 순위에 대한 규칙은 특정 매개변수를 어떤 메서드의 호출에 보내야 할지 판단을 어렵게 만들 수 있다.
그러므로 아주 간단한 경우를 제외하고는 괄호를 사용할 것을 권장한다.

문자열 객체를 만드는 방법에는 여러 가지가 있는데 그중 가장 일반적인 것이 문자열 리터럴을 사용하는 것이다.
문자열 리터럴이란 작은따옴표나 큰따옴표로 묶인 문자의 시퀀스를 말한다.

작은따옴표는 문자열 그대로 사용하고, 큰 따옴표를 사용하면 역슬래시로 시작하는 문자를 찾아서 특정한 이진 값으로 바꾸는 치환 작업을 한다. (예시로 \n는 줄바꿈 문자로 치환됨)
또, 문자열 보간 기능도 있다 문자열 안에 #{expression} 형태가 있다면 expression을 평가한 값으로 변환된다.

루비 메서드에서 반환하는 값은 마지막으로 실행된 표현식의 결괏값이다. 
따라서 메서드를 다음과 같이 표현할 수 있다.

```ruby
# as-is
def say_goodnight(name)
  result = "Good night, #{name.capitalize}"
  return result
end

# to-be
def say_goodnight(name)
  "Good night, #{name.capitalize}"
end
```

루비의 이름으로 용도를 구분할 수 있도록 한 가지 약속을 한다.
이름의 첫 번째 글자가 이 이름이 어떻게 사용될지를 결정한다.
- 지역변수
  - name
  - fish_and_chips
  - _x
  - _26
- 인스턴스 변수
  - @name
  - @point_1
  - @X
  - @_
- 클래스 변수
  - @@total
  - @@N
  - @@x_pos
  - @@SINGLE
- 전역 변수
  - $debug
  - $CUSTOMER
  - $_
  - $Global
- 클래스 이름
  - String
  - ActiveRecord
  - MyClass
- 상수 이름
  - FEET_PER_MILE
  - DEBUG

의미를 갖는 첫 번째 문자 이후에는 알파벳, 숫자, 밑줄(_) 등 어떠한 조합이 와도 상관없다 (단, @ 다음에는 숫자 불가능)
인스턴스 변수는 단어 사이에 밑줄을 넣어서 구분하고(instance_var), 클래스 이름의 경우에는 MixedCase와 같이 각 단어의 첫 글자를 대문자로 한다.
메서드 이름은 ?, !, = 기호로 끝날 수 있다.

### 배열과 해시

루비의 배열과 해시는 색인된 컬렉션이다. 둘다 키를 이용해 접근할 수 있는 객체 모음이다.
해시는 어떠한 객체든 키로 사용 가능하지만 배열은 정수만 사용할 수 있다.
배열과 해시 모두 새로운 요소를 담기 위해 공간이 더 필요하면 스스로 확장한다. (아마 1.5배 혹은 2배씩 늘리는 자료구조 비슷하게 가져갈듯)

```ruby
# 정수, 문자열, 부동소수점으로 표현된 다양한 타입의 구성 요소를 가지는 배열도 생성이 가능하다
a = [ 1, 'cat', 3.14 ]
a[2] = nil
```

많은 언어에서 nil(혹은 null) 아무 객체도 아님을 의미한다.
하지만 루비는 nil 또한 아무것도 아님을 표현하는 하나의 객체일 뿐이다.

단어의 배열을 만들 때, 모든 단어를 따옴표로 묶고 사이사이에 쉼표를 넣는 것 대신 루비에서는 단축 문법인 %w 를 이용해 반복 작업을 피할 수 있다.
```ruby
a = [ 'ant', 'bee', 'cat', 'dog', 'elk' ]
# 다음과 같다
a = %w{ ant bee cat dog elk }
```

해시 리터럴은 대괄호 대신 중괄호를 사용해야 한다.

```ruby
inst_section = {
  'cello' => 'string',
  'clarinet' => 'woodwind',
  'drum' => 'percussion',
  'oboe' => 'woodwind',
  'trumpet' => 'brass',
  'violin' => 'string'
}
```

해시에서 키와 값에는 어떤 객체가 와도 상관없다.
값이 배열이거나 키가 해시인 해시를 만드는 것도 가능하다.
p 메서드는 nil 과 같은 객체도 명시적으로 출력한다

```ruby
p inst_section['oboe'] # => "woodwind"
p inst_section['cello'] # => "string"
p inst_section['bassoon'] # => nil
```

객체가 없을 때 nil 반환을 통해 조건문에서 유용하게 사용할 수 있다.
만약, 기본값을 바꾸고 싶다면

```ruby
histogram = Hash.new(0) # 기본값 0 지정
histogram['ruby'] # => 0
```

### 심벌

```ruby
NORTH = 1
EAST = 2
SOUTH = 3
WEST = 4
```

위와 같은 미리 정의한 상수 대신에 심벌을 사용하면 미리 정의할 필요 없는 동시에 유일한 값이 보장되는 상수 이름이다.

```ruby
walk(:north)
look(:east)
```

심벌에는 값을 직접 부여할 필요없이 루비가 직업 고유한 값을 부여한다.
프로그램의 어디에서 사용하더라도 특정한 이름의 심벌은 항상 같은 값을 가진다.
따라서 아래와 같이 사용할 수 있다.

```ruby
def walk(direction)
  if direction == :north
    # ...
  end
end
```

심벌은 해시의 키로 자주 사용된다. 앞선 해시 리터럴 대신 해시 심볼을 사용할 수 있다.

```ruby
inst_section = {
  :cello => 'string',
  :clarinet => 'woodwind',
  :drum => 'percussion',
  :oboe => 'woodwind',
  :trumpet => 'brass',
  :violin => 'string'
}

inst_section[:oboe] # => "woodwind"
# 문자열과 심벌은 다르다
inst_section['cello'] # => nil
```

루비에서는 해시의 키로 심벌을 사용하는 경우가 많아서 축약 표현도 존재한다.

```ruby
inst_section = {
  cello: 'string',
  clarinet: 'woodwind',
  drum: 'percussion',
  oboe: 'woodwind',
  trumpet: 'brass',
  violin: 'string'
}
```

만약 입력을 받을 때 값이 존재할 때만 수행하고 싶다면 아래와 같은 방식을 사용할 수 있다.
```ruby
while line = gets
  puts line.downcase
end
```

line 변수에 대입한 것이 nil이 아닐때까지 로직을 수행한다.

if나 while 문 안의 내용이 코드 한줄이라면, 이를 짧게 줄여 쓸 수 있는 구문 변경자를 사용할 수 있다.

```ruby
# as-is
if radiation > 3000
  puts "Danger, Will Robinson"
end

# to-be
puts "Danger, Will Robinson" if radiation > 3000
```

```ruby
# as-is
square = 4
while square < 1000
  square = square*square
end

# to-be
square = 4
square = square*square while sqaure < 1000
```

### 정규 표현식

루비는 외부 라이브러리가 아니라 내장 라이브러리를 통해 정규 표현식을 지원한다. (루비, 펄, awk같은 스크립트 언어에서만 기본으로 제공됨)
정규표현식은 문자열에 매치되는 패턴을 기술하는 방법이다.
루비이기 때문에 정규 표현식 또한 객체이다.

=~는 특정 문자열이 정규 표현식과 매치되는지 검사하는데 사용한다.
이 연산자는 문자열에서 패턴이 발견되면 발견된 첫 위치를 반환하며, 그렇지 않을 시 nil을 반환한다.

```ruby
# Perl 혹은 Python을 포함할 시 메시지 출력
line = gets
if line =~ /Perl|Python/
  puts "Scripting language mentioned: #{line}"
end
```

루비의 치환 메서드를 이용하면 정규 표현식으로 찾아낸 문자열의 일부를 다른 문자열로 바꿀 수 있다.
```ruby
line = gets
newline = line.sub(/Perl/, 'Ruby') # 처음 나오는 'Perl'을 'Ruby'로 바꾼다
newerline = newline.gsub(/Python, 'Ruby') # 모든 'Python'을 'Ruby'로 바꾼다
```

### 블록과 반복자

코드 블록은 마치 매개 변수처럼 메서드 호출과 결합할 수 있는 코드다.
코드 블록을 이용해 콜백을 구현할 수 있고, 코드의 일부를 함수에 넘길 수 있고, 반복자를 구현할 수도 있다.

```ruby
{ puts "Hello"}

do
  club.enroll(person)
  person.socialize
end
```

두 가지 모두 코드 블록이다.
중괄호는 do/end 쌍보다 연산자 우선순위가 높다.
루비의 표준 코드 양식에서는 한 줄의 코드 블록은 중괄호로, 여러 줄의 블록은 do/end를 사용한다.

```ruby
# puts "Hi"를 실행하는 블록을 greet 메서드 호출과 결합한다
greet { puts "Hi" }

# 메서드에 매개 변수가 있다면, 블록보다 앞에 써 준다
verbose_greet("Dave", "loyal customer") { puts "Hi" }
```

메서드에서는 루비의 yield 문을 이용하여 결합된 코드 블럭을 여러 차례 실행할 수 있다. (Kotlin의 inline는 오버헤드 없음, 컴파일 시 코드가 직접 삽입되지만 Ruby yield는 런타임에 블록 객체 생성과 호출하여 오버헤드가 존재)

```ruby
# 사용 예시
def call_block
  puts "Start of method" # => "Start of method"
  yield # => "In the block"
  yield # => "In the block"
  puts "End of method" # => "End of method"
end

call_block { puts "In the block" }
```

블록 안에 있는 코드는 yield 호출 시 실행된다.

yield 문에 인자를 적으면 코드 블록에 이 값이 매개 변수로 전달된다.
블록 안에서 이러한 매개 변수를 넘겨받기 위해 |params|와 같이 세로 막대 사이에 매개 변수 이름들을 정의한다.

```ruby
# 메서드가 넘겨받은 블록을 2회 호출하고 이때 블록에 인자를 넘겨준다
def who_says_what
  yield("Dave", "hello")
  yield("Andy", "goodbye")
end

who_says_what {|person, phrase| puts "#{person} says #{phrase}"}

# => Dave says hello
# => Andy says goodbye
```

루비 라이브러리에서는 코드 블록을 반복자(iterator) 구현에 사용된다.
반복자란 배열 등의 집합에서 구성 요소를 하나씩 반환해 주는 함수를 의미한다.

```ruby
animals = %w( ant bee cat dog )
animals.each {|animal| puts animal }
```

C나 자바에서 기본으로 지원하는 반복(제어)문은 루비에서는 단순히 메서드 호출일 뿐이다.

```ruby
# 결합된 블록을 여러 차례 반복 수행
[ 'cat', 'dog', 'horse' ].each {|name| print name, " " }
5.times { print "*" }
3.upto(6) {|i| print i }
('a'..'e').each {|char| print char }
puts
```

### 명령행 인자

루비 프로그램을 명령행에서 실행할 때 인자를 넘겨줄 수 있다.
이렇게 넘겨받은 인자에 접근하는 방법은 ARGV 배열, ARGF를 사용하는 방법이 있다.

```ruby
puts "You gave #{ARGV.size} arguments"
p ARGV
```

```bash
$ ruby cmd_line.rb ant bee cat dog
You gave 4 arguments
["ant", "bee", "cat", "dog"]
```

대부분의 경우 프로그램에서 넘겨지는 인자는 처리하고자 하는 파일들의 이름이 된다.
ARGF는 명령행에서 넘겨진 이름을 가진 모든 파일의 내용을 가지고 있는 특수한 I/O 객체이다. (파일 이름을 넘기지 않으면, 표준 입력 사용)
