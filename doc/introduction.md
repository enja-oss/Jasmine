# introduction-1.3.1.js
Jasmine is a behavior-driven development framework for testing JavaScript code. 
It does not depend on any other JavaScript frameworks. It does not require a DOM. 
And it has a clean, obvious syntax so that you can easily write tests.

JasmineはJavascriptコードをテストするための振舞い駆動開発フレームワークです。
他のJavascriptフレームワークに依存しませんし、DOMも必要としません。
簡単にテストを書くことができるように、きれいで分かりやすい構文を持っています。

This guide is running against Jasmine version 1.3.1 revision 1354556913.

このガイドは1.3.1リビジョン1354556913のバージョンに対して書かれたものです。

##  Suites: describe Your Tests

##  Suites: テストを説明する

A test suite begins with a call to the global Jasmine function describe with two parameters: a string and a function. 
The string is a name or title for a spec suite 
– usually what is under test. The function is a block of code that implements the suite.

テストの組み合わせ（suite）は、Jasmineのグローバル関数 describe へ、文字列と関数の2つのパラメータを渡して呼び出すところから始まります。
文字列は、（通常は）suiteにて何をテストするのかを表す名前かタイトルです。関数はsuiteを実装するコードのブロックです。

## Specs

Specs are defined by calling the global Jasmine function it, which, like describe takes a string and a function. 
The string is a title for this spec and the function is the spec, or test. 
A spec contains one or more expectations that test the state of the code under test.

SpecはJasmineのグローバル関数 it を呼び出して定義します。 
describe と同じように文字列と関数を引数に取ります。
文字列はspecの仕様についてのタイトルであり、関数はspecそのもの、つまりテストコードです。
specには、テスト中の状態を確認するための期待式が1つ以上含まれている必要があります。

An expectation in Jasmine is an assertion that can be either true or false. 
A spec with all true expectations is a passing spec. 
A spec with one or more expectations that evaluate to false is a failing spec.

Jasmineにおける期待式は、trueまたはfalseのいずれかの状態となるアサーションです。
specにおいてすべての期待式がtrueとなる場合、テスト仕様を満たすと判定しますが、
1つ以上の期待式がfalseと評価される場合は満たさないと判定します。

````javascript
// テスト例
describe("A suite", function() {
  // 期待式と一緒にテスト仕様を定義します
  it("contains spec with an expectation", function() {
    expect(true).toBe(true);
  });
});
````

##  It’s Just Functions

##  それは単なる関数です。

Since describe and it blocks are functions, they can contain any executable code necessary to implement the test. JavaScript scoping rules apply, so variables declared in a describe are available to any it block inside the suite.

describe または it ブロックは関数であるため、テストを実施するために必要ないかなるコードも含めることができます。
Javascriptのスコープルールが適用されるため、 describeの中に宣言した変数は、suiteの内側のどの it ブロックからも利用できます。

````javascript
// suiteは単なる関数です
describe("A suite is just a function", function() {
  var a;

  // そしてテスト仕様です
  it("and so is a spec", function() {
    a = true;

    expect(a).toBe(true);
  });
});
````

## Expectations

## 期待式

Expectations are built with the function expect which takes a value, called the actual. 
It is chained with a Matcher function, which takes the expected value.

期待式は、実行値を引数に取る `expect` 関数で構築されます。期待値を渡した `matcher` 関数を連結します。

### Matchers

Each matcher implements a boolean comparison between the actual value and the expected value. 
It is responsible for reporting to Jasmine if the expectation is true or false. 
Jasmine will then pass or fail the spec.

それぞれの matcher は、実行値と期待値をboolean式で返す機能を実装しています。
それは期待式がtrueであるかfalseであるかJasmineに報告する責務を持ち、それによりJasmineは次にspecを渡すか、または失敗するか選択します。

Any matcher can evaluate to a negative assertion by chaining the call to expect with a not before calling the matcher.

すべてmatcherは、期待式とmatcherの間にnotを連結することで、否定の評価を行うことが出来ます。

````javascript
// 'toBe' matcherは===と同等です。
describe("The 'toBe' matcher compares with ===", function() {

  // そして肯定的な評価の場合
  it("and has a positive case ", function() {
    expect(true).toBe(true);
  });

  // そして否定的な評価の場合
  it("and can have a negative case", function() {
    expect(false).not.toBe(true);
  });
});
````

## Included Matchers

## 同梱の Matcher 関数

Jasmine has a rich set of matchers included. Each is used here – all expectations and specs pass.

Jasmineには豊富なmatcherのセットを同梱しています。
すべての期待式はこのページにて使われており、specはすべて合格するようになっています。

There is also the ability to write custom matchers for 
when a project’s domain calls for specific assertions that are not included below.

さらに、プロジェクトの要求にて以下に含まれていない特定のアサーションを使用する場合、カスタムmatchersを書くこともできます。

````javascript
//付属matchers
describe("Included matchers:", function() {

  //'toBe' matcherは===と同等です。
  it("The 'toBe' matcher compares with ===", function() {
    var a = 12;
    var b = a;

    expect(a).toBe(b);
    expect(a).not.toBe(null);
  });

  //'toEqual' matcher
  describe("The 'toEqual' matcher", function() {

    //シンプルなリテラル、変数に使う
    it("works for simple literals and variables", function() {
      var a = 12;
      expect(a).toEqual(12);
    });

    //オブジェクトに使う
    it("should work for objects", function() {
      var foo = {
        a: 12,
        b: 34
      };
      var bar = {
        a: 12,
        b: 34
      };
      expect(foo).toEqual(bar);
    });
  });

  //'toMatch' matcherは正規表現を扱います
  it("The 'toMatch' matcher is for regular expressions", function() {
    var message = 'foo bar baz';

    expect(message).toMatch(/bar/);
    expect(message).toMatch('bar');
    expect(message).not.toMatch(/quux/);
  });

  //'toBeDefined' matcherは`undefined`との比較を行います
  it("The 'toBeDefined' matcher compares against `undefined`", function() {
    var a = {
      foo: 'foo'
    };

    expect(a.foo).toBeDefined();
    expect(a.bar).not.toBeDefined();
  });

  //'toBeUndefined' matcherも`undefined`との比較を行います
  it("The `toBeUndefined` matcher compares against `undefined`", function() {
    var a = {
      foo: 'foo'
    };

    expect(a.foo).not.toBeUndefined();
    expect(a.bar).toBeUndefined();
  });

  //'toBeNull' matcherはnullとの比較を行います
  it("The 'toBeNull' matcher compares against null", function() {
    var a = null;
    var foo = 'foo';

    expect(null).toBeNull();
    expect(a).toBeNull();
    expect(foo).not.toBeNull();
  });

  //'toBeTruthy' matcherはbooleanにキャストした結果との比較を行います
  it("The 'toBeTruthy' matcher is for boolean casting testing", function() {
    var a, foo = 'foo';

    expect(foo).toBeTruthy();
    expect(a).not.toBeTruthy();
  });

  //'toBeFalsy' matcherもbooleanにキャストした結果との比較を行います
  it("The 'toBeFalsy' matcher is for boolean casting testing", function() {
    var a, foo = 'foo';

    expect(a).toBeFalsy();
    expect(foo).not.toBeFalsy();
  });

  //'toContain' matcherは配列の中からアイテムを見つけるために使います
  it("The 'toContain' matcher is for finding an item in an Array", function() {
    var a = ['foo', 'bar', 'baz'];

    expect(a).toContain('bar');
    expect(a).not.toContain('quux');
  });

  //'toBeLessThan' matcherは数学的な比較を行います
  it("The 'toBeLessThan' matcher is for mathematical comparisons", function() {
    var pi = 3.1415926, e = 2.78;

    expect(e).toBeLessThan(pi);
    expect(pi).not.toBeLessThan(e);
  });

  //'toBeGreaterThan' matcherも数学的な比較を行います
  it("The 'toBeGreaterThan' is for mathematical comparisons", function() {
    var pi = 3.1415926, e = 2.78;

    expect(pi).toBeGreaterThan(e);
    expect(e).not.toBeGreaterThan(pi);
  });

  //'toBeCloseTo' matcherは与えられたレベルにおける十進精度の比較を行います
  it("The 'toBeCloseTo' matcher is for precision math comparison", function() {
    var pi = 3.1415926, e = 2.78;

    expect(pi).not.toBeCloseTo(e, 2);
    expect(pi).toBeCloseTo(e, 0);
  });

  //'toThrow' matcherはfunctionが例外を発生させる場合のテストに用います
  it("The 'toThrow' matcher is for testing if a function throws an exception", function() {
    var foo = function() {
      return 1 + 2;
    };
    var bar = function() {
      return a + 1;
    };

    expect(foo).not.toThrow();
    expect(bar).toThrow();
  });
});
````

## Grouping Related Specs with describe

## 関連するSpecをグループ化 describe

The describe function is for grouping related specs. 
The string parameter is for naming the collection of specs, 
and will be concatenated with specs to make a spec’s full name. 
This aids in finding specs in a large suite. 
If you name them well, your specs read as full sentences in traditional BDD style.

`describe` 関数は関連するspecをグループ化するためのものです。
文字列の引数はspecの集合を命名するためのものであり、関連するspecをつなぎ合わせた完全な名前を設定します。
これは、巨大なSuiteの中でspecを見つけるのに役立ちます。
specにふさわしい名前付けを行った場合、従来の[BDD](http://en.wikipedia.org/wiki/Behavior-driven_development)スタイルの完全文として読むことができます。

````javascript
describe("A spec", function() {

  //これは単なる関数なので、いかなるコードも定義することが出来ます
  it("is just a function, so it can contain any code", function() {
    var foo = 0;
    foo += 1;

    expect(foo).toEqual(1);
  });
  
  //1つ以上の期待式を含めることができます
  it("can have more than one expectation", function() {
    var foo = 0;
    foo += 1;

    expect(foo).toEqual(1);
    expect(true).toEqual(true);
  });
});
````

## Setup and Teardown

## Setup と Teardown

To help a test suite DRY up any duplicated setup and teardown code, 
Jasmine provides the global beforeEach and afterEach functions. 
As the name implies the beforeEach function is called once before each spec in the describe 
is run and the afterEach function is called once after each spec.

いかなるsetupとteardownコードの重複から、suiteのDRY原則を支援するため、
Jasmineではグローバル関数 `beforeEach` と `afterEach` を提供しています。
名前が示すように、 `beforeEach` は `describe` の中の各specが実行される前に一度だけ呼び出されます。
また、 `afterEach` は各specの後に一度だけ呼び出されます。

Here is the same set of specs written a little differently. 
The variable under test is defined at the top-level scope — the describe block — and initialization code 
is moved into a beforeEach function. 
The afterEach function resets the variable before continuing.

ここに、少し違ったように書かれたspecの同じセットがあります。
最上位のスコープにある `describe` ブロックに以下のテストの変数が定義され、初期化するコードが `beforeEach` 関数内にあります。
そして次のspecが継続される前に、 `afterEach` 関数にて変数がリセットされます。

````javascript
//spec（setupとtear-downを伴った）
describe("A spec (with setup and tear-down)", function() {
  var foo;

  beforeEach(function() {
    foo = 0;
    foo += 1;
  });

  afterEach(function() {
    foo = 0;
  });

  //これは単なる関数なので、いかなるコードも定義することが出来ます
  it("is just a function, so it can contain any code", function() {
    expect(foo).toEqual(1);
  });

  //1つ以上の期待式を含めることができます
  it("can have more than one expectation", function() {
    expect(foo).toEqual(1);
    expect(true).toEqual(true);
  });
});
````

## Nesting describe Blocks

## describe ブロックのネスト

Calls to describe can be nested, with specs defined at any level. 
This allows a suite to be composed as a tree of functions. 
Before a spec is executed, Jasmine walks down the tree executing each beforeEach function in order. 
After the spec is executed, Jasmine walks through the afterEach functions similarly.

`describe` 呼び出しは、いかなるのレベルで定義されたspecと共にネストすることができます。
これにより、suiteをツリー構造のようにすることができます。
specを実行する前に、Jasmineはツリーを順に下って行き `beforeEach` を実行します。
そしてspecが実行された後、 `afterEach` も同様に実行します。

````javascript
describe("A spec", function() {
  var foo;

  beforeEach(function() {
    foo = 0;
    foo += 1;
  });

  afterEach(function() {
    foo = 0;
  });

  //これは単なる関数なので、いかなるコードも定義することが出来ます
  it("is just a function, so it can contain any code", function() {
    expect(foo).toEqual(1);
  });

  //1つ以上の期待式を含めることができます
  it("can have more than one expectation", function() {
    expect(foo).toEqual(1);
    expect(true).toEqual(true);
  });

  //内側にネストした番目のdescribe
  describe("nested inside a second describe", function() {
    var bar;

    beforeEach(function() {
      bar = 1;
    });

    //必要に応じて両方のスコープを参照できます
    it("can reference both scopes as needed ", function() {
      expect(foo).toEqual(bar);
    });
  });
});
````

## Disabling Specs and Suites

## Specs と Suites を無効にする

Suites and specs can be disabled with the xdescribe and xit functions, respectively. 
These suites and specs are skipped when run and thus their results will not appear in the results.

suiteやspecは、それぞれ `xdescribe` と `xit` とすることで無効にすることができます。
これらのsiuteやspecは実行時にスキップされ、最終的な結果に影響することはありません。

````javascript
xdescribe("A spec", function() {
  var foo;

  beforeEach(function() {
    foo = 0;
    foo += 1;
  });

  //これは単なる関数なので、いかなるコードも定義することが出来ます
  xit("is just a function, so it can contain any code", function() {
    expect(foo).toEqual(1);
  });
});
````

## Spies

Jasmine’s test doubles are called spies. A spy can stub any function and tracks calls to it and all arguments. 
There are special matchers for interacting with spies.

Jasmineのテストを構成するもうひとつの部品はspyと呼ばれています。
spyは任意の関数のスタブとなることができ、その呼び出しやすべての引数を追跡することができます。
そしてspyと相互に作用する特別なmatcherがあります

The toHaveBeenCalled matcher will return true if the spy was called. 
The toHaveBeenCalledWith matcher will return true if the argument list matches any of the recorded calls to the spy.

spyが呼び出された場合、 `toHaveBeenCalled` はtrueを返します。
また、 `toHaveBeenCalledWith` は、引数リストがspyに記録されたいづれかの呼び出しと一致した場合、trueを返します。

````javascript
describe("A spy", function() {
  var foo, bar = null;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      }
    };

    spyOn(foo, 'setBar');

    foo.setBar(123);
    foo.setBar(456, 'another param');
  });

  //spyが呼び出されたことを追跡します
  it("tracks that the spy was called", function() {
    expect(foo.setBar).toHaveBeenCalled();
  });

  //呼び出し回数を追跡します
  it("tracks its number of calls", function() {
    expect(foo.setBar.calls.length).toEqual(2);
  });

  //呼び出す際のすべての引数を追跡します
  it("tracks all the arguments of its calls", function() {
    expect(foo.setBar).toHaveBeenCalledWith(123);
    expect(foo.setBar).toHaveBeenCalledWith(456, 'another param');
  });

  //最も直近に呼び出されたものにアクセスすることもできます
  it("allows access to the most recent call", function() {
    expect(foo.setBar.mostRecentCall.args[0]).toEqual(456);
  });

  //（直近だけでなく）任意の呼び出しもアクセスすることができます
  it("allows access to other calls", function() {
    expect(foo.setBar.calls[0].args[0]).toEqual(123);
  });

  //関数上ですべての実行を停止します
  it("stops all execution on a function", function() {
    expect(bar).toBeNull();
  });
});
````

## Spies: andCallThrough

By chaining the spy with andCallThrough, 
the spy will still track all calls to it but in addition it will delegate to the actual implementation.

spyに `andCallThrough` を連結することで、spyはすべての呼び出しを追跡しますが、指定されたもの処理は本来の実装へ委譲されます。

````javascript
//call throughで構成されるspy
describe("A spy, when configured to call through", function() {
  var foo, bar, fetchedBar;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      },
      getBar: function() {
        return bar;
      }
    };

    spyOn(foo, 'getBar').andCallThrough();

    foo.setBar(123);
    fetchedBar = foo.getBar();
  });

  //spyが呼び出されたことを追跡します
  it("tracks that the spy was called", function() {
    expect(foo.getBar).toHaveBeenCalled();
  });

  //他の機能の影響を受けてはならない
  it("should not effect other functions", function() {
    expect(bar).toEqual(123);
  });

  //呼び出された場合、要求された値が返されます
  it("when called returns the requested value", function() {
    expect(fetchedBar).toEqual(123);
  });
});
````

## Spies: andReturn

By chaining the spy with andReturn, all calls to the function will return a specific value.

spyに `andReturn` を連結することで、すべての呼び出しは指定した値を返します。

````javascript
//戻り値を偽装したspy
describe("A spy, when faking a return value", function() {
  var foo, bar, fetchedBar;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      },
      getBar: function() {
        return bar;
      }
    };

    spyOn(foo, 'getBar').andReturn(745);

    foo.setBar(123);
    fetchedBar = foo.getBar();
  });

  //spyが呼び出されたことを追跡します
  it("tracks that the spy was called", function() {
    expect(foo.getBar).toHaveBeenCalled();
  });

  //他の機能の影響を受けてはならない
  it("should not effect other functions", function() {
    expect(bar).toEqual(123);
  });

  //呼び出された場合、要求された値が返されます
  it("when called returns the requested value", function() {
    expect(fetchedBar).toEqual(745);
  });
});
````

## Spies: andCallFake

By chaining the spy with andCallFake, all calls to the spy will delegate to the supplied function.

spyに `andCallFake` を連結することで、すべてのspyへの呼び出しは指定された関数へ委譲されます。

````javascript
//戻り値を偽装したspy
describe("A spy, when faking a return value", function() {
  var foo, bar, fetchedBar;

  beforeEach(function() {
    foo = {
      setBar: function(value) {
        bar = value;
      },
      getBar: function() {
        return bar;
      }
    };

    spyOn(foo, 'getBar').andCallFake(function() {
      return 1001;
    });

    foo.setBar(123);
    fetchedBar = foo.getBar();
  });

  //spyが呼び出されたことを追跡します
  it("tracks that the spy was called", function() {
    expect(foo.getBar).toHaveBeenCalled();
  });

  //他の機能の影響を受けてはならない
  it("should not effect other functions", function() {
    expect(bar).toEqual(123);
  });

  //呼び出された場合、要求された値が返されます
  it("when called returns the requested value", function() {
    expect(fetchedBar).toEqual(1001);
  });
});
````

## Spies: createSpy

When there is not a function to spy on, jasmine.createSpy can create a “bare” spy. 
This spy acts as any other spy – tracking calls, arguments, etc. 
But there is no implementation behind it. Spies are JavaScript objects and can be used as such.

上のspyの機能で足りない場合、 `jasmine.createSpy` で空のスパイを作成することができます。
このspyは（呼び出しや引数の追跡など）他のどのspy役もこなします。
しかし、この状態ではまだ何も実装されていません。
spyはJavascriptのオブジェクトであり、同様に扱うことができます。

````javascript
//独自で作成したspy
describe("A spy, when created manually", function() {
  var whatAmI;

  beforeEach(function() {
    whatAmI = jasmine.createSpy('whatAmI');

    whatAmI("I", "am", "a", "spy");
  });

  //これはエラーレポートのための名称です
  it("is named, which helps in error reporting", function() {
    expect(whatAmI.identity).toEqual('whatAmI')
  });

  //spyが呼び出されたことを追跡します
  it("tracks that the spy was called", function() {
    expect(whatAmI).toHaveBeenCalled();
  });

  //呼び出し回数を追跡します
  it("tracks its number of calls", function() {
    expect(whatAmI.calls.length).toEqual(1);
  });

  //呼び出す際のすべての引数を追跡します
  it("tracks all the arguments of its calls", function() {
    expect(whatAmI).toHaveBeenCalledWith("I", "am", "a", "spy");
  });

  //最も直近に呼び出されたものにアクセスすることもできます
  it("allows access to the most recent call", function() {
    expect(whatAmI.mostRecentCall.args[0]).toEqual("I");
  });
});
````

## Spies: createSpyObj

In order to create a mock with multiple spies, use jasmine.createSpyObj and pass an array of strings. 
It returns an object that has a property for each string that is a spy.

複数のspyを持ったmockを作成するために、文字列の配列を渡して `jasmine.createSpyObj` を使います。
それぞれの文字列がspyのプロパティとなったオブジェクトを返します。

````javascript
//複数のspyを独自で作成した場合
describe("Multiple spies, when created manually", function() {
  var tape;

  beforeEach(function() {
    tape = jasmine.createSpyObj('tape', ['play', 'pause', 'stop', 'rewind']);

    tape.play();
    tape.pause();
    tape.rewind(0);
  });

  //要求された機能ごとにspyを作成します
  it("creates spies for each requested function", function() {
    expect(tape.play).toBeDefined();
    expect(tape.pause).toBeDefined();
    expect(tape.stop).toBeDefined();
    expect(tape.rewind).toBeDefined();
  });

  //spyが呼び出されたことを追跡します
  it("tracks that the spies were called", function() {
    expect(tape.play).toHaveBeenCalled();
    expect(tape.pause).toHaveBeenCalled();
    expect(tape.rewind).toHaveBeenCalled();
    expect(tape.stop).not.toHaveBeenCalled();
  });

  //呼び出す際のすべての引数を追跡します
  it("tracks all the arguments of its calls", function() {
    expect(tape.rewind).toHaveBeenCalledWith(0);
  });
});
````

##  Matching Anything with jasmine.any

jasmine.any takes a constructor or “class” name as an expected value. It returns true if the constructor matches the constructor of the actual value.

````javascript
describe("jasmine.any", function() {
  it("matches any value", function() {
    expect({}).toEqual(jasmine.any(Object));
    expect(12).toEqual(jasmine.any(Number));
  });

  describe("when used with a spy", function() {
    it("is useful for comparing arguments", function() {
      var foo = jasmine.createSpy('foo');
      foo(12, function() {
        return true
      });

      expect(foo).toHaveBeenCalledWith(jasmine.any(Number), jasmine.any(Function));
    });
  });
});
````

##  Mocking the JavaScript Clock

The Jasmine Mock Clock is available for a test suites that need the ability to use setTimeout or setInterval callbacks. It makes the timer callbacks synchronous, thus making them easier to test.

It is installed with a call to jasmine.Clock.useMock in a spec or suite that needs to call the timer functions.

Calls to any registered callback are triggered when the clock is ticked forward via the jasmine.Clock.tick function, which takes a number of milliseconds.

````javascript
describe("Manually ticking the Jasmine Mock Clock", function() {
  var timerCallback;

  beforeEach(function() {
    timerCallback = jasmine.createSpy('timerCallback');
    jasmine.Clock.useMock();
  });

  it("causes a timeout to be called synchronously", function() {
    setTimeout(function() {
      timerCallback();
    }, 100);

    expect(timerCallback).not.toHaveBeenCalled();

    jasmine.Clock.tick(101);

    expect(timerCallback).toHaveBeenCalled();
  });

  it("causes an interval to be called synchronously", function() {
    setInterval(function() {
      timerCallback();
    }, 100);

    expect(timerCallback).not.toHaveBeenCalled();

    jasmine.Clock.tick(101);
    expect(timerCallback.callCount).toEqual(1);

    jasmine.Clock.tick(50);
    expect(timerCallback.callCount).toEqual(1);

    jasmine.Clock.tick(50);
    expect(timerCallback.callCount).toEqual(2);
  });
});
````

##  Asynchronous Support

Jasmine also has support for running specs that require testing asynchronous operations.

Specs are written by defining a set of blocks with calls to runs, which usually finish with an asynchronous call.

The waitsFor block takes a latch function, a failure message, and a timeout.

The latch function polls until it returns true or the timeout expires, whichever comes first. If the timeout expires, the spec fails with the error message.

Once the asynchronous conditions have been met, another runs block defines final test behavior. This is usually expectations based on state after the asynch call returns.

````javascript
describe("Asynchronous specs", function() {
  var value, flag;

  it("should support async execution of test preparation and exepectations", function() {
  
  runs(function() {
      flag = false;
      value = 0;

      setTimeout(function() {
        flag = true;
      }, 500);
    });

    waitsFor(function() {
      value++;
      return flag;
    }, "The Value should be incremented", 750);

    runs(function() {
      expect(value).toBeGreaterThan(0);
    });
  });
});
````

##  The Runner and Reporter

Jasmine is built in JavaScript and must be included into a JS environment, such as a web page, in order to run. Like this web page.

This file is written in JavaScript and is compiled into HTML via Rocco. The JavaScript file is then included, via a <script> tag, so that all of the above specs are evaluated and recorded with Jasmine. Thus Jasmine can run all of these specs. This page is then considered a ‘runner.’

Scroll down the page to see the results of the above specs. All of the specs should pass.

Meanwhile, here is how a runner works to execute a Jasmine suite.

Create the HTMLReporter, which Jasmine calls to provide results of each spec and each suite. The Reporter is responsible for presenting results to the user.

Delegate filtering of specs to the reporter. Allows for clicking on single suites or specs in the results to only run a subset of the suite.

Run all of the tests when the page finishes loading – and make sure to run any previous onload handler

````javascript
(function() {
  var jasmineEnv = jasmine.getEnv();
  jasmineEnv.updateInterval = 250;

  var htmlReporter = new jasmine.HtmlReporter();
  jasmineEnv.addReporter(htmlReporter);

  jasmineEnv.specFilter = function(spec) {
    return htmlReporter.specFilter(spec);
  };

  var currentWindowOnload = window.onload;
  window.onload = function() {
    if (currentWindowOnload) {
      currentWindowOnload();
    }

    document.querySelector('.version').innerHTML = jasmineEnv.versionString();
    execJasmine();
  };

  function execJasmine() {
    jasmineEnv.execute();
  }
})();
````

## Test Results

Scroll down to see the results of all of these specs.

##  Downloads

The Standalone Release is for simple, browser page, or console projects
The Jasmine Ruby Gem is for Rails, Ruby, or Ruby-friendly development
Other Environments are supported as well
Support

Mailing list at Google Groups – a great first stop to ask questions, propose features, or discuss pull requests
Report Issues at Github
The Backlog lives at Pivotal Tracker
Follow @JasmineBDD on Twitter
Thanks

Running documentation inspired by @mjackson and the 2012 Fluent Summit.
