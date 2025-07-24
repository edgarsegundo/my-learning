Ótima pergunta! Vamos aplicar o modo **estrito** (`'use strict'`) no exemplo de **ligação implícita** e entender o que muda — e como **preservar o `this`** corretamente.

---

### ⚠️ Problema: perda do `this` ao atribuir o método a uma variável

```javascript
'use strict';

const myObject = {
  name: "Objeto",
  greet: function() {
    console.log(`Olá, eu sou ${this.name}`);
  }
};

const anotherGreet = myObject.greet;
anotherGreet(); 
// Em modo estrito: this === undefined → Saída: "Olá, eu sou undefined"
```

👉 Nesse caso, mesmo que `greet` tenha sido definido dentro de `myObject`, ao fazer `anotherGreet = myObject.greet`, você **perde a ligação implícita**. A função é chamada "solta", então o `this` não aponta mais para `myObject`.

---

## ✅ Como resolver? Três maneiras:

### ✅ 1. **Usar `.bind()` para fixar `this`:**

```javascript
'use strict';

const myObject = {
  name: "Objeto",
  greet: function() {
    console.log(`Olá, eu sou ${this.name}`);
  }
};

const anotherGreet = myObject.greet.bind(myObject);
anotherGreet(); // Saída: Olá, eu sou Objeto
```

> `.bind(myObject)` cria uma nova função onde `this` está permanentemente atrelado a `myObject`.

---

### ✅ 2. **Chamar diretamente como método (ligação implícita):**

```javascript
myObject.greet(); // Saída: Olá, eu sou Objeto
```

> Aqui, como a função é chamada com `myObject.` antes, o `this` funciona corretamente mesmo com `'use strict'`.

---

### ✅ 3. **Usar função arrow que preserva `this` léxico (se aplicável):**

Se estiver trabalhando em contexto onde a função `greet` é definida dentro de outro escopo (como uma classe ou função de fábrica), você pode usar uma arrow function que herda o `this` do escopo em que foi criada:

```javascript
'use strict';

function createObject(name) {
  return {
    name,
    greet: () => {
      console.log(`Olá, eu sou ${name}`);
    }
  };
}

const myObject = createObject("Objeto");
myObject.greet(); // Olá, eu sou Objeto
```

> Atenção: **Arrow functions não têm `this` próprio**, então só use essa abordagem se for realmente herdar de fora — **não é substituto direto** para métodos que dependem de `this`.

Sim, **nesse caso específico**, `call` e `apply` têm **o mesmo efeito** — ambos invocam a função imediatamente com o `this` explicitamente definido como `person`. A **única diferença** entre eles está **em como os argumentos são passados**.

---

### ✅ Diferença principal:

| Método    | Chama a função? | Como recebe argumentos?                                  |
| --------- | --------------- | -------------------------------------------------------- |
| `call()`  | Sim             | Passa argumentos separadamente: `f(arg1, arg2, ...)`     |
| `apply()` | Sim             | Passa **um array** de argumentos: `f([arg1, arg2, ...])` |
| `bind()`  | **Não** chama   | Retorna nova função com `this` fixo                      |

---

### 🧪 Exemplo com argumentos:

```javascript
function introduce(greeting, punctuation) {
  console.log(`${greeting}, eu sou ${this.name}${punctuation}`);
}

const person = { name: "Alice" };

// Usando call:
introduce.call(person, "Olá", "!"); 
// Saída: Olá, eu sou Alice!

// Usando apply:
introduce.apply(person, ["Oi", "."]); 
// Saída: Oi, eu sou Alice.
```

### ✅ Então, **tanto faz usar `call` ou `apply`** quando:

* Você **não está passando argumentos** (como no seu exemplo).
* Ou quando você **já tem os argumentos no formato certo** (separados ou array).

---

### Quando `apply` pode ser útil:

* Quando você tem um array de argumentos, como ao repassar valores:

  ```javascript
  const args = [1, 2, 3];
  someFunction.apply(obj, args);
  ```

---

Se quiser, posso mostrar um caso onde **`apply` é mais prático**, como ao usar `Math.max` com arrays. Quer ver esse exemplo?
