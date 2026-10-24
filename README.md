# Respostas — Os Quatro Pilares da POO (TypeScript)

---

## 1. Herança

```typescript
class Pessoa {

    constructor(public nome: string) {}

}

class Aluno extends Pessoa {

    constructor(nome: string, public matricula: string) {
        super(nome);
    }

}

class Professor extends Pessoa {

    constructor(nome: string, public disciplina: string) {
        super(nome);
    }

}

const aluno = new Aluno("Maria", "2024001");
const professor = new Professor("Carlos", "Matemática");

console.log(aluno.nome, aluno.matricula);
console.log(professor.nome, professor.disciplina);
```

---

## 2. Encapsulamento

```typescript
class ContaBancaria {

    private saldo: number = 0;

    depositar(valor: number): boolean {
        if (valor > 0) {
            this.saldo += valor;
            return true;
        }
        return false;
    }

    sacar(valor: number): boolean {
        if (valor > 0 && valor <= this.saldo) {
            this.saldo -= valor;
            return true;
        }
        return false;
    }

    consultarSaldo(): number {
        return this.saldo;
    }

}

const conta = new ContaBancaria();
conta.depositar(500);
conta.sacar(200);
console.log(conta.consultarSaldo()); // 300

// conta.saldo = 999999; // Erro: 'saldo' é privado
```

---

## 3. Polimorfismo

```typescript
class Animal {
    emitirSom(): void {
        console.log("Som");
    }
}

class Cachorro extends Animal {
    override emitirSom(): void {
        console.log("Au Au");
    }
}

class Gato extends Animal {
    override emitirSom(): void {
        console.log("Miau");
    }
}

const animais: Animal[] = [new Cachorro(), new Gato()];

for (const animal of animais) {
    animal.emitirSom();
}
```

---

## 4. Abstração

```typescript
abstract class Forma {
    abstract calcularArea(): number;
}

class Retangulo extends Forma {

    constructor(private largura: number, private altura: number) {
        super();
    }

    calcularArea(): number {
        return this.largura * this.altura;
    }

}

class Circulo extends Forma {

    constructor(private raio: number) {
        super();
    }

    calcularArea(): number {
        return Math.PI * this.raio ** 2;
    }

}

const formas: Forma[] = [new Retangulo(4, 5), new Circulo(3)];

for (const forma of formas) {
    console.log(forma.calcularArea().toFixed(2));
}
```

---

## 5. Projeto integrando os quatro pilares

| Pilar | Onde foi aplicado |
|-------|-------------------|
| Herança | `Aluno` e `Professor` herdam de `Pessoa` |
| Encapsulamento | `_nome` e `_nota` são privados, com `get` e `set` validados |
| Polimorfismo | `apresentar()` é implementado de forma diferente em `Aluno` e `Professor` |
| Abstração | Interface `Apresentavel` e classe abstrata `Pessoa` |

```typescript
interface Apresentavel {
    apresentar(): void;
}

abstract class Pessoa implements Apresentavel {

    private _nome: string;

    constructor(nome: string) {
        this._nome = nome;
    }

    get nome(): string {
        return this._nome;
    }

    set nome(novoNome: string) {
        if (novoNome.trim() === "") {
            throw new Error("O nome não pode ser vazio.");
        }
        this._nome = novoNome.trim();
    }

    abstract apresentar(): void;

}

class Aluno extends Pessoa {

    private _nota: number = 0;

    constructor(nome: string, nota: number) {
        super(nome);
        this.nota = nota;
    }

    get nota(): number {
        return this._nota;
    }

    set nota(valor: number) {
        if (valor < 0 || valor > 10) {
            throw new Error("A nota deve estar entre 0 e 10.");
        }
        this._nota = valor;
    }

    apresentar(): void {
        console.log(`Aluno: ${this.nome} | Nota: ${this.nota}`);
    }

}

class Professor extends Pessoa {

    constructor(nome: string, public disciplina: string) {
        super(nome);
    }

    apresentar(): void {
        console.log(`Professor: ${this.nome} | Disciplina: ${this.disciplina}`);
    }

}

const pessoas: Apresentavel[] = [
    new Aluno("Maria", 8.5),
    new Professor("Carlos", "Matemática"),
];

for (const pessoa of pessoas) {
    pessoa.apresentar();
}
```
