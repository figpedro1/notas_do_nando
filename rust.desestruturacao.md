# Desestruturação no rust

A desestruturação no rust é muito parecida com a desestruturação em linguagens
como javascript. A sintaxe básica é:

```
let minha_tupla = (15, "Desestruturando!", true); // Criando a tupla que vamos desestruturar

let (valor, string, boolean) = minha_tupla; // Desestruturando a tupla!
```

Note que no caso de tuplas a desestruturação é feita utilizando a ordem dos
elementos. Esse não é o caso com estruturas:

```

struct MinhaStruct {
        // Declarando a struct
        primeiro_campo: i32,
        segundo_campo: String,
        terceiro_campo: bool,
}

let minha_struct = MinhaStruct {
    // Declarando uma variável do tipo MinhaStruct
    primeiro_campo: 50i32,
    segundo_campo: String::from("Hello, World!"),
    terceiro_campo: false,
};

let MinhaStruct {
    segundo_campo,
    terceiro_campo,
    primeiro_campo,
} = minha_struct; // Desestruturando a minha_struct!
```

Nesse caso podemos perceber que primeiramente: a ordem dos campos que desejamos
desestruturar não importa, precisamos apenas utilizar o nome correto, e em
segundo lugar, notamos que objetos nomeados, sejam eles enums ou structs
necessitam ter o seu nome antes da dita desestruturação. Além disso também
podemos perceber que usamos "{}" para selecionar os campos que desejamos.

Podemos notar que cada tipo de objeto utiliza a o símbolo que foi usado na sua
declaração para os desestruturarem (Tuplas utilizam "()", structs utilizam "{}",
arrays utilizam "[]", etc).
