---
title: PHPUnit 메소드 알아보기
date: 2019-01-10 14:32:02
tags:
---


안녕하세요.
오늘은 지난 시간에 포스팅한 [유닛 테스트를 시작해보자](https://njhyuk.github.io/2019/01/10/phpunit-start/)에 이어, PHPUnit의 많이 사용하는 메소드 몇가지를 알아 보려고 합니다.

## assertTrue
전달한 $condition 이 true 인지 체크 합니다.

```
assertTrue(bool $condition[, string $message = ''])
```

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class MyTest extends TestCase
{
    public function testExample()
    {
        $this->assertTrue(true);
    }
}
```

assertFalse() 메소드는 이와 정반대의 역할을 수행합니다.

## assertEmpty
전달한 $actual 의 empty 여부를 체크 합니다.

```
assertEmpty(mixed $actual[, string $message = ''])
```

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class MyTest extends TestCase
{
    public function testExample()
    {
        $stack = [];
        $this->assertEmpty($stack);
    }
}
```

assertNotEmpty() 메소드는 이와 정반대의 역할을 수행합니다.

## assertEquals
테스트 결과로 예측한 $expected가 $actual과 같은 결과값인지 체크 합니다.

```
assertEquals(mixed $expected, mixed $actual[, string $message = ''])
```

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class MyTest extends TestCase
{
    public function testExample()
    {
        $this->assertEquals(1, 1);
    }
}
```

assertNotEquals() 메소드는 이와 정반대의 역할을 수행합니다.

## assertSame
테스트 결과로 예측한 $expected가 $actual과 같은 결과값인지 체크 합니다.
**assertEquals 는 두 값이 같은지를 비교하나, assertSame은 비교할 두 대상이 같은 주소값인지 비교합니다.**

```
assertSame(mixed $expected, mixed $actual[, string $message = ''])
```

```php
<?php
use PHPUnit\Framework\TestCase;

class SameTest extends TestCase
{
    public function testFailure()
    {
        $this->assertSame('2204', 2204);
    }
}
?>
```

assertNotSame() 메소드는 이와 정반대의 역할을 수행합니다.

## assertInstanceOf
$actual이 테스트 결과로 예측한 $expected 의 인스턴스인지 체크 합니다.

```
assertInstanceOf($expected, $actual[, $message = ''])
```

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;

class MyTest extends TestCase
{
    public function testExample()
    {
        $this->assertInstanceOf(MyTest::class, new MyTest);
    }
}
```

assertNotInstanceOf() 메소드는 이와 정반대의 역할을 수행합니다.

더 자세한 내용은 공식 문서인 [PHPUnit Manual — PHPUnit 7.4 Manual](https://phpunit.readthedocs.io/) 문서를 확인 하시기 바랍니다.
감사합니다.