---
title: Брз почеток
---

<Intro>

Добредојдовте во документацијата за React! Оваа страница ви дава вовед во 80% од концептите на React што ќе ги користите секојдневно.

</Intro>

<YouWillLearn>

- Како да создавате и вгнездувате компоненти
- Како да додадете обележување (markup) и стилови
- Како да прикажувате податоци
- Како да рендерирате услови и списоци
- Како да одговарате на настани и да го ажурирате екранот
- Како да споделувате податоци помеѓу компоненти

</YouWillLearn>

## Создавање и вгнездување на компоненти {/*components*/}

React апликациите се изградени од *компоненти*. Компонентата е дел од корисничкиот интерфејс (UI) што има сопствена логика и изглед. Може да биде мала како копче или голема како цела страница.

React компонентите се JavaScript функции што враќаат обележување:

```js
function MyButton() {
  return (
    <button>Јас сум копче</button>
  );
}
```

Откако ќе ја декларирате `MyButton`, можете да ја вгнездите во друга компонента:

```js {5}
export default function MyApp() {
  return (
    <div>
      <h1>Добредојдовте во мојата апликација</h1>
      <MyButton />
    </div>
  );
}
```

Забележете дека `<MyButton />` започнува со голема буква. Така знаете дека е React компонента. Имињата на React компонентите секогаш мора да започнуваат со голема буква, додека HTML ознаките мора да бидат со мали букви.

Погледнете го резултатот:

<Sandpack>

```js
function MyButton() {
  return (
    <button>
      Јас сум копче
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Добредојдовте во мојата апликација</h1>
      <MyButton />
    </div>
  );
}
```

</Sandpack>

Клучните зборови `export default` ја одредуваат главната компонента во фајлот. Ако не сте запознаени со дел од синтаксата на JavaScript, [MDN](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) и [javascript.info](https://javascript.info/import-export) имаат одлични референци.

## Пишување обележување со JSX {/*writing-markup-with-jsx*/}

Синтаксата за обележување што ја видовте погоре се вика *JSX*. Не е задолжителна, но повеќето React проекти користат JSX заради погодноста. Сите [алатки што ги препорачуваме за локален развој](/learn/installation) поддржуваат JSX веднаш.

JSX е построг од HTML. Мора да ги затворите ознаките како `<br />`. Компонентата не може да врати повеќе JSX ознаки истовремено. Треба да ги ставите во заеднички родител, како `<div>...</div>` или празна `<>...</>` обвивка (фрагмент):

```js {3,6}
function AboutPage() {
  return (
    <>
      <h1>За нас</h1>
      <p>Здраво.<br />Како сте?</p>
    </>
  );
}
```

Ако имате многу HTML што треба да го пренесете во JSX, може да користите [онлајн конвертор.](https://transform.tools/html-to-jsx)

## Додавање стилови {/*adding-styles*/}

Во React, CSS класата ја одредувате со `className`. Работи на истиот начин како HTML атрибутот [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class):

```js
<img className="avatar" />
```

Потоа ги пишувате CSS правилата за неа во посебен CSS фајл:

```css
/* Во вашиот CSS */
.avatar {
  border-radius: 50%;
}
```

React не пропишува како да додавате CSS фајлови. Во наједноставниот случај, ќе додадете [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) ознака во вашиот HTML. Ако користите алатка за градење или оквир, консултирајте се со документацијата за да научите како да додадете CSS фајл во проектот.

## Прикажување податоци {/*displaying-data*/}

JSX ви овозможува да ставите обележување во JavaScript. Кадравите загради ви дозволуваат повторно да „влезете“ во JavaScript за да вградите променлива од вашиот код и да ја прикажете на корисникот. На пример, ова ќе прикаже `user.name`:

```js {3}
return (
  <h1>
    {user.name}
  </h1>
);
```

Можете исто така да „избегате“ во JavaScript од JSX атрибутите, но мора да користите кадрави загради *наместо* наводници. На пример, `className="avatar"` ја пренесува низата `"avatar"` како CSS класа, но `src={user.imageUrl}` ја чита вредноста на JavaScript променливата `user.imageUrl` и потоа ја пренесува како атрибут `src`:

```js {3,4}
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

Можете да ставите и посложени изрази во JSX кадравите загради, на пример [спојување низи](https://javascript.info/operators#string-concatenation-with-binary):

<Sandpack>

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://react.dev/images/docs/scientists/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Слика на ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

```css
.avatar {
  border-radius: 50%;
}

.large {
  border: 4px solid gold;
}
```

</Sandpack>

Во горниот пример, `style={{}}` не е посебна синтакса, туку обичен објект `{}` внатре во `style={ }` во JSX кадравите загради. Можете да го користите атрибутот `style` кога вашите стилови зависат од JavaScript променливи.

## Условен приказ {/*conditional-rendering*/}

Во React нема посебна синтакса за пишување услови. Наместо тоа, ќе ги користите истите техники како при обичен JavaScript. На пример, може да користите наредба [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) за условно вклучување на JSX:

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

Ако претпочитате покомпактен код, може да го користите [условниот оператор `?`.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) За разлика од `if`, работи и внатре во JSX:

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

Кога не ви треба гранката `else`, може да користите пократка [логичка синтакса со `&&`.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation)

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

Сите овие пристапи важат и за условно задавање на атрибути. Ако некои од овие JavaScript конструкции не ви се познати, почнете со тоа секогаш да користите `if...else`.

## Прикажување на списоци {/*rendering-lists*/}

Ќе се потпрете на можностите на JavaScript како [`for` јамката](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) и [функцијата `map()` на низи](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) за да рендерирате списоци од компоненти.

На пример, да речеме дека имате низа производи:

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

Внатре во компонентата, користете ја функцијата `map()` за да ја претворите низата производи во низа `<li>` елементи:

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

Забележете дека `<li>` има атрибут `key`. За секој елемент во списокот треба да пренесете низа или број што единствено го одредува тој елемент меѓу „браќата“ во дрвото. Обично, клучот доаѓа од вашите податоци, на пример од ID во база. React ги користи клучевите за да знае што се случило ако подоцна вметнете, избришете или прередите елементи.

<Sandpack>

```js
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

</Sandpack>

## Одговарање на настани {/*responding-to-events*/}

Можете да одговарате на настани со декларирање на функции за *ракување со настани* внатре во вашите компоненти:

```js {2-4,7}
function MyButton() {
  function handleClick() {
    alert('Ме кликнавте!');
  }

  return (
    <button onClick={handleClick}>
      Кликни ме
    </button>
  );
}
```

Забележете дека `onClick={handleClick}` нема загради на крајот! Не ја *повикувајте* функцијата за ракување со настан: доволно е да ја *пренесете*. React ќе ја повика таа функција кога корисникот ќе го кликне копчето.

## Ажурирање на екранот {/*updating-the-screen*/}

Често ќе сакате компонентата да „запомни“ некоја информација и да ја прикаже. На пример, можеби сакате да броите колку пати е кликнато копче. За тоа додадете *состојба (state)* во компонентата.

Прво, увезете [`useState`](/reference/react/useState) од React:

```js
import { useState } from 'react';
```

Потоа може да декларирате *променлива за состојба* внатре во компонентата:

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

Од `useState` добивате две работи: моменталната состојба (`count`) и функцијата што ја ажурира (`setCount`). Можете да им дадете било кои имиња, но конвенцијата е `[нешто, setНешто]`.

Првиот пат кога копчето се прикажува, `count` ќе биде `0` затоа што на `useState()` му пренесовте `0`. Кога сакате да ја смените состојбата, повикајте `setCount()` и пренесете ја новата вредност. Кликот на ова копче ќе го зголеми бројачот:

```js {5}
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Кликнато {count} пати
    </button>
  );
}
```

React повторно ќе ја повика вашата функција за компонента. Овој пат, `count` ќе биде `1`. Потоа `2`. И така натаму.

Ако ја рендерирате истата компонента повеќе пати, секоја добива сопствена состојба. Кликнете го секое копче посебно:

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Бројачи што се ажурираат одделно</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Кликнато {count} пати
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

Забележете како секое копче „си ја памети“ сопствената состојба `count` и не влијае на другите копчиња.

## Користење на Hooks {/*using-hooks*/}

Функциите што започнуваат со `use` се викаат *Hooks*. `useState` е вграден Hook што го нуди React. Други вградени Hooks ги наоѓате во [API референцата.](/reference/react) Можете и сами да пишете Hooks со комбинирање на постоечките.

Hooks се построги од другите функции. Можете да ги повикувате само *на врвот* на вашите компоненти (или на други Hooks). Ако сакате `useState` во услов или јамка, издвојте нова компонента и ставете го таму.

## Споделување податоци помеѓу компоненти {/*sharing-data-between-components*/}

Во претходниот пример, секое `MyButton` имаше сопствен независен `count`, и при клик само `count` на тоа копче се менуваше:

<DiagramGroup>

<Diagram name="sharing_data_child" height={367} width={407} alt="Диаграм со дрво од три компоненти: родител MyApp и две деца MyButton. И двете MyButton имаат бројач со вредност нула.">

На почетокот, состојбата `count` на секое `MyButton` е `0`

</Diagram>

<Diagram name="sharing_data_child_clicked" height={367} width={407} alt="Истиот диаграм како претходниот: бројачот на првото дете MyButton е истакнат по клик и е зголемен на еден. Второто MyButton уште е нула." >

Првото `MyButton` го ажурира својот `count` на `1`

</Diagram>

</DiagramGroup>

Но, често ви треба компонентите да *ги споделуваат податоците и секогаш да се ажурираат заедно*.

За двете `MyButton` да прикажуваат ист `count` и да се ажурираат заедно, треба да ја „подигнете“ состојбата од поединечните копчиња до најблиската компонента што ги содржи сите.

Во овој пример, тоа е `MyApp`:

<DiagramGroup>

<Diagram name="sharing_data_parent" height={385} width={410} alt="Диаграм: родител MyApp и две деца MyButton. MyApp има count нула пренесена надолу кон двете MyButton, кои исто така покажуваат нула." >

На почетокот, состојбата `count` на `MyApp` е `0` и се пренесува кон двете деца

</Diagram>

<Diagram name="sharing_data_parent_clicked" height={385} width={410} alt="Истиот диаграм: count на родителот MyApp е истакнат по клик и е еден. Протокот кон двете MyButton е истакнат; и двете деца покажуваат еден." >

По клик, `MyApp` ја ажурира состојбата `count` на `1` и ја пренесува надолу кон двете деца

</Diagram>

</DiagramGroup>

Сега, кога ќе кликнете било кое копче, `count` во `MyApp` ќе се смени, што ќе ги смени и двата броја во `MyButton`. Еве како да го изразите тоа во код.

Прво, *подигнете ја состојбата* од `MyButton` во `MyApp`:

```js {2-6,18}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Бројачи што се ажурираат одделно</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... го преместуваме кодот оттука ...
}

```

Потоа, *пренесете ја состојбата надолу* од `MyApp` кон секое `MyButton`, заедно со заедничкиот ракувач за клик. Информацијата до `MyButton` ја пренесувате со JSX кадрави загради, како што претходно правевте со вградени ознаки како `<img>`:

```js {11-12}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Бројачи што се ажурираат заедно</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

Информацијата што вака ја пренесувате се вика _props_. Сега компонентата `MyApp` ја содржи состојбата `count` и ракувачот `handleClick`, и *ги пренесува двете како props* до секое копче.

На крај, сменете го `MyButton` за да ги *чита* props што ги пратил родителот:

```js {1,3}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Кликнато {count} пати
    </button>
  );
}
```

Кога кликнувате, се активира ракувачот `onClick`. На секое копче, prop-от `onClick` беше поставен на `handleClick` од `MyApp`, па се извршува тој код. Тој код повикува `setCount(count + 1)`, што ја зголемува променливата за состојба `count`. Новата вредност на `count` се пренесува како prop до секое копче, па сите ја покажуваат новата вредност. Ова се вика „подигнување на состојба“ (*lifting state up*). Со подигнување на состојбата, ја споделивте меѓу компонентите.

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Бројачи што се ажурираат заедно</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Кликнато {count} пати
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

## Следни чекори {/*next-steps*/}

Досега ги знаете основите на пишување React код!

Погледнете го [туторијалот](/learn/tutorial-tic-tac-toe) за да ги ставите во пракса и да ја изградите вашата прва мини-апликација со React.
