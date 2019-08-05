---
title: PHPUnit 1부 - 유닛 테스트를 시작 해보자
---

{% asset_img logo.png source : https://codeburst.io/writing-php-unit-testing-friendly-code-5ef9a8a49da1 %}

어플리케이션은 매우 방대하게 이루어져 있습니다.
보기에는 간단한 기능을 추가 하더라도, 사용자 테스트를 진행하게 된다면 사용자는 개발자가 예상한 사용자의 접근 플로우와는 다르게 전혀 예상치 못한 방법으로 접근 하게 되고 오류가 발생하기 마련입니다.
이러한 문제를 해결하기 위해 오늘은 유닛 테스트에 대해 알아봅시다.

## 유닛 테스트

위키 백과 에서는 유닛테스트를 다음과 같이 설명하고 있습니다.

> 유닛 테스트(unit test)는 컴퓨터 프로그래밍에서 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차다.
즉, 모든 함수와 메소드에 대한 테스트 케이스(Test case)를 작성하는 절차를 말한다.
이를 통해서 언제라도 코드 변경으로 인해 문제가 발생할 경우, 단시간 내에 이를 파악하고 바로 잡을 수 있도록 해준다. 이상적으로, 각 테스트 케이스는 서로 분리되어야 한다.
이를 위해 가짜 객체(Mock object)를 생성하는 것도 좋은 방법이다.
유닛 테스트는 (일반적인 테스트와 달리) 개발자(developer) 뿐만 아니라 보다 더 심도있는 테스트를 위해 테스터(tester)에 의해 수행되기도 한다.
쉽게 얘기해서 기능을 추가할때마다 작성한 유닛 테스트를 통해, 추가한 기능이 기존 기능들에 영향을 미쳐 오류가 발생하고 있는지 자동으로 테스트 할 수 있는 것입니다.

## 시작하기

예제로 php의 phpunit을 활용하여 Laravel 프레임워크의 예제로 시작 해 봅시다.
우선적으로 phpunit 설치가 준비되어야 합니다.

## 테스트 케이스 생성
새로운 테스트 케이스를 생성하려면 make:test 아티즌 명령어를 입력 합니다.

```
php artisan make:test UserTest
```

## 코드 작성

명령어로 생성된 클래스에, 성공하는 케이스를 작성 해 보겠습니다.

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use Illuminate\Foundation\Testing\WithoutMiddleware;
use Illuminate\Foundation\Testing\DatabaseMigrations;
use Illuminate\Foundation\Testing\DatabaseTransactions;

class UserTest extends TestCase
{
    /**
     * A basic test example.
     *
     * @return void
     */
    public function testExample()
    {
        $this->assertTrue(true);
    }
}
```

## 실행

다음의 명령어를 통해 작성한 케이스를 실행 할 수 있습니다.

```
./vendor/bin/phpunit --bootstrap vendor/autoload.php Tests/Feature/UserTest
```

## 테스트 결과

테스트에 성공하면 다음과 같은 결과를 반환합니다.

{% asset_img success.png [success] %}

그렇다면 테스트에 실패하면 어떻게 표현될까요? 다음과 같이 실패한 코드와 결과가 화면에 출력됩니다.

{% asset_img fail.png [fail] %}

## 마지막으로..
오늘은 간단한 예제를 통해 단위테스트의 개념과 테스트 케이스를 생성하고 실행하는 법에 대해 알아보았습니다. 실제로 활용하기에 오늘의 내용으로는 아직 부족하지만, 소프트웨어 테스트를 위한 자동화에 대해 고민해 보게 된 시간 이였으면 좋겠습니다.
다음에 기회가 되면 phpunit의 기능에 대해 상세하게 알아보도록 하겠습니다.
감사합니다.