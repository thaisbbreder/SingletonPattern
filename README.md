# Singleton
O Singleton é um padrão de projeto **criacional** que permite garantir que uma classe tenha **apenas uma instância**, enquanto fornece um **ponto de acesso global** para essa instância.

![image](https://github.com/user-attachments/assets/345e587f-2b48-4797-b987-1ef7e018406b)


### Todas as implementações do Singleton tem esses dois passos em comum:

1. Fazer o construtor padrão privado, para prevenir que outros objetos usem o operador new com a classe singleton.
2. Criar um método estático de criação que age como um construtor. Esse método chama o construtor privado por debaixo dos panos para criar um objeto e o salva em um campo estático. Todas as chamadas seguintes para esse método retornam o objeto em cache.
Se o seu código tem acesso à classe singleton, então ele será capaz de chamar o método estático da singleton. Então sempre que aquele método é chamado, o mesmo objeto é retornado.

### **Aplicabilidade**
- Utilize o padrão Singleton quando uma classe em seu programa deve ter apenas uma instância disponível para todos seus clientes; por exemplo, um objeto de base de dados único compartilhado por diferentes partes do programa.<br>
O padrão Singleton desabilita todos os outros meios de criar objetos de uma classe exceto pelo método especial de criação. Esse método tanto cria um novo objeto ou retorna um objeto existente se ele já tenha sido criado.

- Utilize o padrão Singleton quando você precisa de um controle mais estrito sobre as variáveis globais.<br>
 Ao contrário das variáveis globais, o padrão Singleton garante que há apenas uma instância de uma classe. Nada, a não ser a própria classe singleton, pode substituir a instância salva em cache.

## 2. FORMAS DE IMPLEMENTAR UM SINGLETON
### 2.1 Singleton com Campo Público Estático

   ```java
public class GerenciadorDeJanelas {
    public static final GerenciadorDeJanelas INSTANCE = new GerenciadorDeJanelas();

    private GerenciadorDeJanelas() {
    }
}
 ```

 O construtor privado é chamado apenas uma única vez para inicializar o campo final estático INSTANCE. Como não temos um construtor público ou protegido, temos a garantia de que existirá apenas uma instância para a classe GerenciadorDeJanelas.<br>

Para burlar isso e criar uma segunda instância dessa classe, um cliente poderia chamar o construtor privado reflexivamente com a ajuda do método AcessibleObject.setAcessible. Para defender-se deste ataque, o construtor teria que ser modificado para lançar uma exceção caso fosse solicitado a criar uma segunda instância.

### 2.2 Singleton com Método de Fábrica Estático

   ```java
public class GerenciadorDeJanelas {
    private static final GerenciadorDeJanelas INSTANCE = new GerenciadorDeJanelas();

    private GerenciadorDeJanelas() {
    }

    public static GerenciadorDeJanelas getInstance() {
        return INSTANCE;
    }
}
 ```
Todas as chamadas a GerenciadorDeJanelas.getInstance() retornam a mesma referência de objeto. A grande vantagem desta abordagem é que através de um campo público, torna-se claro que esta classe é um Singleton. Outra vantagem dessa abordagem é se quisermos retornar uma instância diferente para cada chamada, ou seja, mudar completamente o comportamento. Nesse tipo de implementação, alterar esse comportamento seria bastante simples.<br>

O grande problema das duas abordagens anteriores aparece se quisermos torna-las serializáveis, apenas adicionar implements Singleton não é o suficiente. Para preservar a propriedade Singleton, teríamos que tornar todos os campos de instância transientes e fornecer um método readResolve. Caso contrário, sempre que desserializarmos uma instância teríamos uma nova instância criada, para evitar isso deveríamos criar o método

   ```java
private Object readResolve() {
    return INSTANCE;
}
 ```
### 2.3 SINGLETON COM ENUM
Enumerações são tipos de campos que consistem em um conjunto fixo de constantes (static final), sendo como uma lista de valores pré-definidos.
 ```java
public enum GerenciadorDeJanelas {
    INSTANCE;
}
 ```
Essa abordagem é equivalente à abordagem de campo público, no entanto é considerada muito mais clara e concisa, além disso, seu mecanismo de desserialização é fornecido mais facilmente e possui uma garantia sólida contra instanciação múltipla. O Enum é a melhor maneira de implementar um Singleton em Java.
   
## 3.**Prós e contras**
- Você pode ter certeza que uma classe só terá uma única instância.
- Você ganha um ponto de acesso global para aquela instância.
- O objeto singleton é inicializado somente quando for pedido pela primeira vez.
 -Viola o princípio de responsabilidade única. O padrão resolve dois problemas de uma só vez.
- O padrão Singleton pode mascarar um design ruim, por exemplo, quando os componentes do programa sabem muito sobre cada um.
- O padrão requer tratamento especial em um ambiente multithreaded para que múltiplas threads não possam criar um objeto singleton várias vezes.
- Pode ser difícil realizar testes unitários do código cliente do Singleton porque muitos frameworks de teste dependem de herança quando produzem objetos simulados. Já que o construtor da classe singleton é privado e sobrescrever métodos estáticos é impossível na maioria das linguagem, você terá que pensar em uma maneira criativa de simular o singleton. Ou apenas não escreva os testes. Ou não use o padrão Singleton.
