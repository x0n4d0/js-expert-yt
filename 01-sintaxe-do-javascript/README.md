<div align='justify'>

# **Sintaxe do JavaScript**

## Atributos Privados no NodeJS (v14+)

A partir da versão 14 do NodeJS, passou a ser permitido escrever atributos privados, utilizando apenas o '#' antes da variável ou método dentro da classe.

```js
class EntityBase {
  #name 
  #age 
  #gender 
  constructor({ name, age, gender }) {
    this.#name = name;
    this.#age = age;
    this.#gender = gender;

    this.#metodo();
  }

  #metodo() {
    ...
  }

  get name() {
    return this.#name;
  }

  set name(name) {
    this.#name = name;
  }
}
```

## Membros Estáticos

```js
class Employee extends EntityBase {
  
  static #TAXES_PERCENTUAL = 0.2;
  
  ...
}
```

## Internacionalização (Intl) - Utilizado Para Formatar Moeda

```js
const value = 2000.50;

Intl.NumberFormat('pt-br', {
  currency: 'BRL',
  style: 'currency'
}).format(value);

// R$ 2.000,00
```

## Módulo Interno de Testes do NodeJS (assert)

O NodeJS possui internamente o módulo 'assert' para escrever testes automatizados. Uma coisa interessante do assert, é que ele não vai reportar quando houver sucesso, ou seja, quando os testes passarem, não será retornado nada no terminal. Alguns exemplos de métodos do assert:

### assert.throws

Verificando o lançamento de exceção com o método throws:

```js
// Uma classe qualquer, nesse exemplo não vamos passar o age no construtor e tentar chamar o método birthYear:

class Employee {
  #name 
  #age 
  #gender 
  constructor({ name, age, gender }) {
    this.#name = name;
    this.#age = age;
    this.#gender = gender;
  }

  get birthYear() {
    if (!this.#age) {
      throw new Error('[!] You must define age first!');
    }

    return new Date().getFullYear - this.#age;
  }
}

const employee = new Employee({
  name: 'XuxaDaSilva',
  gender: GENDER.female
});

// A mensagem de erro deverá ser exatamente a mesma que foi lançada no erro!
assert.throws(() => employee.birthYear, { message: '[!] You must define age first!' });
```

### assert.deepStrictEqual

Verificando se um valor é igual ao outro:

```js
  const employee = new Employee({
    name: 'Joãozinho',
    age: 20,
    gender: GENDER.male,
  });

  assert.deepStrictEqual(employee.name, 'Mr. Joãozinho');
```

Uma coisa que você deve tomar cuidado na hora de utilizar o deepStrictEqual do NodeJS, é que quando você vai comparar strings, em nível de referência pode não bater e retornar erro. Por exemplo:

```js
assert.deepStrictEqual(employee.grossPay, 'R$ 5.000,40');
```

Por mais que o valor de employee.grossPay seja exatamente 'R$ 5.000,40', em nível de referência o JavaScript não vai considerar igual, logo, irá retornar erro (que não é igual). Para corrigir esse erro de referência, no código foi utilizado essa estratégia:

```js
assert.deepStrictEqual(employee.grossPay, Util.formatCurrency(5000.40));
```

Uma coisa importante quando for escrever testes automatizados que envolvam datas ou timer, é lembrar que quando vira o ano/dia/mês ou a hora, os seus testes podem quebrar! Para isso não acontecer, você deve "mockar", realizar um mock. Nesse caso, nós "mockamos" o método getFullYear do JavaScript, fixando a data para 2021 no arquivo de testes, para que ao passar dos anos os testes não quebrem.

```js
const CURRENT_YEAR = 2021;
Date.prototype.getFullYear = () => CURRENT_YEAR;

const expectedBirthYear = 2001;
assert.deepStrictEqual(employee.birthYear, expectedBirthYear);
```


## Contexto do JavaScript

O JavaScript permite que você simplesmente abra chaves para definir um contexto, fazendo com que seja criado um escopo de bloco dentro do arquivo. Nesse exemplo, declaramos duas variáveis utilizando o `const` com o mesmo nome e como elas estão em escopos diferentes, não deu nenhum problema.

```js
{
  const employee = null;
}

{
  const employee = null;
}
```

</div>