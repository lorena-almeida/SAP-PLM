// Classe base (superclasse) - Animal
class Animal {
    // Encapsulamento: variáveis privadas e acesso via getters e setters
    private String nome;
    private int idade;

    public Animal(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }

    // Getter e Setter para 'nome'
    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    // Getter e Setter para 'idade'
    public int getIdade() {
        return idade;
    }

    public void setIdade(int idade) {
        this.idade = idade;
    }

    // Método comum
    public void emitirSom() {
        System.out.println("O animal faz um som.");
    }
}

// Classe derivada (subclasse) - Cachorro, que herda de Animal
class Cachorro extends Animal {

    public Cachorro(String nome, int idade) {
        super(nome, idade);  // Chamando o construtor da superclasse
    }

    // Polimorfismo: Sobrescrevendo o método 'emitirSom'
    @Override
    public void emitirSom() {
        System.out.println("O cachorro late!");
    }
}

// Outra classe derivada - Gato
class Gato extends Animal {

    public Gato(String nome, int idade) {
        super(nome, idade);  // Chamando o construtor da superclasse
    }

    // Polimorfismo: Sobrescrevendo o método 'emitirSom'
    @Override
    public void emitirSom() {
        System.out.println("O gato mia!");
    }
}

// Classe principal para executar o código
public class Main {

    public static void main(String[] args) {
        // Criando instâncias de Cachorro e Gato
        Animal cachorro = new Cachorro("Rex", 3);
        Animal gato = new Gato("Whiskers", 2);

        // Utilizando métodos polimórficos
        System.out.println(cachorro.getNome() + " tem " + cachorro.getIdade() + " anos.");
        cachorro.emitirSom();

        System.out.println(gato.getNome() + " tem " + gato.getIdade() + " anos.");
        gato.emitirSom();
    }
}
