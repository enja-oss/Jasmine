# introduction-1.3.1.js

JasmineはJavascriptコードをテストするための振舞い駆動開発フレームワークです。
他のJavascriptフレームワークに依存しませんし、DOMも必要としません。
簡単にテストを書くことができるように、きれいで分かりやすい構文を持っています。

このガイドは1.3.1リビジョン1354556913のバージョンに対して書かれたものです。

##  Suites: テストを説明する

テストの組み合わせ（suite）は、Jasmineのグローバル関数 describe へ、文字列と関数の2つのパラメータを渡して呼び出すところから始まります。
文字列は、（通常は）suiteにて何をテストするのかを表す名前かタイトルです。関数はsuiteを実装するコードのブロックです。

## Specs

SpecはJasmineのグローバル関数 it を呼び出して定義します。 
describe と同じように文字列と関数を引数に取ります。
文字列はspecの仕様についてのタイトルであり、関数はspecそのもの、つまりテストコードです。
specには、テスト中の状態を確認するための期待式が1つ以上含まれている必要があります。

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

##  それは単なる関数です。

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

## 期待式

期待式は、実行値を引数に取る `expect` 関数で構築されます。期待値を渡した `matcher` 関数を連結します。

### Matchers

それぞれの matcher は、実行値と期待値をboolean式で返す機能を実装しています。
それは期待式がtrueであるかfalseであるかJasmineに報告する責務を持ち、それによりJasmineは次にspecを渡すか、または失敗するか選択します。

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

## 同梱の Matcher 関数

Jasmineには豊富なmatcherのセットを同梱しています。
すべての期待式はこのページにて使われており、specはすべて合格するようになっています。

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

## 関連するSpecをグループ化 describe

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

## Setup と Teardown

setupとteardownのコードがテストSuiteでDRY原則に従えるよう、
Jasmineではグローバル関数 `beforeEach` と `afterEach` を提供しています。
名前が示すように、 `beforeEach` は `describe` の中の各specが実行される前に一度だけ呼び出されます。
また、 `afterEach` は各specの後に一度だけ呼び出されます。

ここに、少し異なるように書かれたspecの同じセットがあります。
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

## describe ブロックのネスト

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

## Specs と Suites を無効にする

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

Jasmineにおけるテストダブルはspyと呼ばれています。
spyは任意の関数のスタブとなることができ、その呼び出しやすべての引数を追跡することができます。
そしてspyと相互に作用する特別なmatcherがあります

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

## jasmine.anyで任意の値をマッチング

`jasmine.any` は期待値としてコンストラクタかクラス名を引数に取ります。
実行値のコンストラクタと一致する場合、trueを返します。


````javascript
describe("jasmine.any", function() {
  //任意の値とマッチング
  it("matches any value", function() {
    expect({}).toEqual(jasmine.any(Object));
    expect(12).toEqual(jasmine.any(Number));
  });

  //spyと一緒に使用する場合
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

## JavaScript タイマーMockを作成する

JasmineのMock Clockは、テストsuiteが `setTimeout` や `setInterval` のコールバックを必要とする場合にそれを可能にします。
タイマーのコールバックが同期するようになるので、テストが容易になります。

タイマー関数を呼び出すspecやsuiteの中で、`jasmine.Clock.useMock` を呼び出すことで導入できます。

`jasmine.Clock.tick` 関数に渡されたミリ秒ごとに経過時間がチェックされ、登録されているコールバックを呼び出します。

````javascript
//Jasmine Mock Clockで手動タイマー
describe("Manually ticking the Jasmine Mock Clock", function() {
  var timerCallback;

  beforeEach(function() {
    timerCallback = jasmine.createSpy('timerCallback');
    jasmine.Clock.useMock();
  });

  //timeoutが同期して呼び出されている
  it("causes a timeout to be called synchronously", function() {
    setTimeout(function() {
      timerCallback();
    }, 100);

    expect(timerCallback).not.toHaveBeenCalled();

    jasmine.Clock.tick(101);

    expect(timerCallback).toHaveBeenCalled();
  });

  //intervalが同期して呼び出されている
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

## 非同期のサポート

またJasmineでは、非同期処理をテストする必要があるspecの実行もサポートしています。

specは `runs` 呼び出しを伴うブロックにて定義されます。通常、非同期呼び出しと共に終了します。

`waitsFor` ブロックはlatch回路関数、エラーメッセージ、タイムアウトを引数に取ります。

latch回路関数は、tureかタイムアウトのいずれかが返されるまでポーリングします。
タイムアウトが発生した場合、specは障害メッセージとともに失敗します。

一度、非同期の条件が満たされれば、他の `runs` ブロックについてもfinal（不変な）テスト動作として定義できます。
これは通常、非同期処理終了後の状態が期待結果となります。

````javascript
//非同期spec
describe("Asynchronous specs", function() {
  var value, flag;

  //非同期テストの準備と結果予測をサポートしています
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

## RunnerおよびReporter

JasmineはJavascriptで構築され、実行するためにWebページなどのJS実行環境に含まれる必要があります。
例えばこのWebページのように。

このファイルはJavascriptで記述され、[Rocco](http://rtomayko.github.com/rocco/)でHTMLにコンパイルされます。 
Javascriptファイルはこのとき、`<script>` タグ内に含まれ、その結果、上記すべてのspecはJasmineで評価され記録されます。
従って、Jasmineはこれらのspecをすべて実行することができるのです。
つまりこのページは  &lsquo;runner&lsquo; と考えられます。

ページをスクロールダウンして上記のspecの結果を表示してください。
もちろん、すべてのspecが合格しているはずです。

これらが、runnerがJasmineのsuiteを実行する仕組みです。

`HTMLReporter` を作成します。Jasmineはspecやsuiteごとの結果を提供するためにこれを呼び出します。
このReporterは結果をユーザに提示する責務を持ちます。

Reporterにフィルタリングするspecを渡してください。
単一のsuiteやspecをクリックすることで、suiteの一部分のみを実行して結果を取得することができます。

ページのロードが完了した際に、すべてのテストを実行します。
そして、 `onload` イベントハンドラの前に実行するようにしてください。

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

## テスト結果

これらすべてのspec結果を見るためにスクロールダウンしてください。

##  Downloads

* [Standalone Release](http://github.com/pivotal/jasmine/downloads) はシンプル、ブラウザページ、コンソールプロジェクト用です。
* [Jasmine Ruby Gem](http://github.com/pivotal/jasmine-gem) は、Rails, Ruby, or Rubyに慣れている開発者用です。
* [他の環境](http://github.com/pivotal/jasmine/wiki)も同様にサポートされます。

## Support

* [Mailing list](http://groups.google.com/group/jasmine-js) はGoogle Groupにあります。機能の提案、pull requestsの議論、質問する素晴らしい場です。
* バグの報告は[Guthub](http://github.com/pivotal/jasmine/issues)へ。
* [Backlog](http://www.pivotaltracker.com/projects/10606)は[Pivotal Tracker](http://www.pivotaltracker.com/)にあります。
* Twitterで[@JasmineBDD](http://twitter.com/jasminebdd)をフォローしてください。

## Thanks

このランニングドキュメントは[@mjackson](http://twitter.com/mjackson)と2012年の[Fluent](http://fluentconf.com/) Summitからインスパイアされたものです。
