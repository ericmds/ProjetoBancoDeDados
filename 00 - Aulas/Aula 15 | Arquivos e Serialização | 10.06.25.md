# Arquivos
## Exemplo 1
### Classe Aluno
```java
package Ex1;

public class Aluno {
	private String nome;
	private int idade;

	public Aluno(String nome, int idade) {
		this.nome = nome;
		this.idade = idade;
	}

	public String getNome() {
		return nome;
	}

	public int getIdade() {
		return idade;
	}

}
```

### Classe PrincipalLeitura
```java
package Ex1;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class PrincicipalLeitura {

	public static void main(String[] args) {
		// Lendo os alunos do arquivo
		try {
			FileReader arquivo = new FileReader("alunos.txt");
			BufferedReader leitor = new BufferedReader(arquivo);

			System.out.println("Alunos lidos do arquivo: ");

			String linha;
			while ((linha = leitor.readLine()) != null) {
				String[] campos = linha.split(",");

				String nome = campos[0];
				int idade = Integer.parseInt(campos[1]);

				Aluno aluno = new Aluno(nome, idade);

				System.out.println("Nome: " + aluno.getNome() + ", Idade: " + aluno.getIdade());
			}

			leitor.close();
			arquivo.close();
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

}
```

### Classe PrincipalEscritura
```java
package Ex1;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class PrincipalEscritura {

	public static void main(String[] args) {

		Aluno a1 = new Aluno("Vanessa", 22);
		Aluno a2 = new Aluno("João", 20);
		Aluno a3 = new Aluno("Maria", 19);

		try {
			// Quando tem true depois do arquivo, ele ativa o append e não sobreescreve o arquivo
			FileWriter arquivo = new FileWriter("alunos.txt", true);
			// FileWriter arquivo = new FileWriter("alunos.txt"); Assim sobreescreve
			BufferedWriter escritor = new BufferedWriter(arquivo);

			escritor.write(a1.getNome() + "," + a1.getIdade());
			escritor.newLine();

			escritor.write(a2.getNome() + "," + a2.getIdade());
			escritor.newLine();

			escritor.write(a3.getNome() + "," + a3.getIdade());
			escritor.newLine();

			escritor.close();
			arquivo.close();

		} catch (IOException e) {
			e.printStackTrace();
		}

	}

}
```

## Exemplo 2
### Classe Arquivo
```java
package Ex2;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import Ex1.Aluno;

public class Arquivo {
	private FileWriter arqw;
	private BufferedWriter escritor;
	private FileReader arqr;
	private BufferedReader leitor;
	private List<Aluno> listAlunos;
	public String nomeArquivo;

	public Arquivo(String nomeArquivo) {
		this.nomeArquivo = nomeArquivo;
		listAlunos = new ArrayList<>();
	}

	public void gravaArquivo(Aluno a) {
		// Escrevendo os alunos em um arquivo de texto
		try {
			// Escrevendo os alunos no arquivo
			arqw = new FileWriter(nomeArquivo + ".txt", true);
			escritor = new BufferedWriter(arqw);
			escritor.write(a.getNome() + "," + a.getIdade());
			escritor.newLine();
			escritor.close();
			arqw.close();

			System.out.println("Alunos salvos no arquivo alunos.txt");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public List<Aluno> leArquivo() {
		// Lendo os alunos do arquivo
		System.out.println("Alunos lidos do arquivo");
		try {
			arqr = new FileReader(nomeArquivo + ".txt");
			leitor = new BufferedReader(arqr);
			String linha;
			while ((linha = leitor.readLine()) != null) {
				String[] campos = linha.split(",");

				String nome = campos[0];
				int idade = Integer.parseInt(campos[1]); // Converte a idade que está como string para int
				Aluno aluno = new Aluno(nome, idade);
				listAlunos.add(aluno);
			}

			leitor.close();
			arqr.close();

		} catch (IOException e) {
			e.printStackTrace();
		}

		return listAlunos;
	}
}
```

### Classe Principal
```java
package Ex2;

import java.util.ArrayList;
import java.util.List;

import Ex1.Aluno;

public class Principal {

	public static void main(String[] args) {
		// Criando objetos Aluno
		Aluno aluno1 = new Aluno("Éric", 24);
		Aluno aluno2 = new Aluno("Vanessa", 22);
		Aluno aluno3 = new Aluno("Bernardo", 22);
		List<Aluno> lista = new ArrayList<>();
		Arquivo arquivo = new Arquivo("alunos");
		arquivo.gravaArquivo(aluno1);
		arquivo.gravaArquivo(aluno2);
		arquivo.gravaArquivo(aluno3);

		lista = arquivo.leArquivo();

		for (Aluno a : lista) {
			System.out.println("Nome: " + a.getNome() + ", Idade: " + a.getIdade());
		}
	}
}
```

# Serialização
## Exemplo 1 | Serialização
### Classe Produto
```java
package Ex1;

import java.io.Serializable;

public class Produto implements Serializable {
	private String codigo;
	private String nome;
	private double preco;
	private transient String temporario;

	public Produto(String codigo, String nome, double preco, String temporario) {
		this.codigo = codigo;
		this.nome = nome;
		this.preco = preco;
		this.temporario = temporario;
	}

	public String getCodigo() {
		return codigo;
	}

	public String getNome() {
		return nome;
	}

	public double getPreco() {
		return preco;
	}

	public String getTemporario() {
		return temporario;
	}

	@Override
	public String toString() {
		return "Produto [Código = " + codigo + ", Nome = " + nome + ", Preco = " + preco + "]";
	}
}
```

Principal
```java
package Ex1;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class Principal {

	public static void main(String[] args) {
		Produto produto = new Produto("ABC123", "Exemplo de Produto", 9.99, "Campo temporário");

		// Serialização
		try {
			FileOutputStream arquivoSaida = new FileOutputStream("produto.ser");
			ObjectOutputStream objetoSaida = new ObjectOutputStream(arquivoSaida);

			objetoSaida.writeObject(produto);
			objetoSaida.close();
			arquivoSaida.close();

			System.out.println("Objeto serializado e salvo em produto.ser");
		} catch (IOException e) {
			e.printStackTrace();
		}

		// Desserialização
		try {
			FileInputStream arquivoEntrada = new FileInputStream("produto.ser");
			ObjectInputStream objetoEntrada = new ObjectInputStream(arquivoEntrada);

			Produto produtoDesserializado = (Produto) objetoEntrada.readObject();
			objetoEntrada.close();
			arquivoEntrada.close();

			System.out.println("Objeto desserializado: " + produtoDesserializado); // Chama o método sobrescrito
																					// toString();
			System.out.println("Vai apresentar NULL: " + produtoDesserializado.getTemporario());
		} catch (IOException | ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
}
```

## Exemplo 2 | JSON
### Classe Pessoa
```java
package Ex2;

public class Pessoa {
	private String nome;
	private int idade;

	public Pessoa(String nome, int idade) {
		this.nome = nome;
		this.idade = idade;
	}

	public String getNome() {
		return nome;
	}

	public int getIdade() {
		return idade;
	}

	@Override
	public String toString() {
		return "Pessoa [Nome = " + nome + ", Idade = " + idade + "]";
	}
}
```

### Classe Principal
```java
package Ex2;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

public class Principal {

	public static void main(String[] args) {
		// Criando um objeto para serealizar
		Pessoa p = new Pessoa("Éric", 24);

		// Convertendo o objeto em um JSONObject
		JSONObject json = new JSONObject();
		json.put("Nome", p.getNome());
		json.put("Idade", p.getIdade());
		String jsonString = json.toJSONString();

		gravaArquivo(jsonString);

		try {
			lerArquivo();
		} catch (org.json.simple.parser.ParseException e) {
			e.printStackTrace();
		}
	}

	public static void gravaArquivo(String jsonString) {

		try (FileWriter fileWriter = new FileWriter("pessoa.json")) {
			fileWriter.write(jsonString);
			System.out.println("Arquivo person.json salvo com sucesso.");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public static void lerArquivo() throws org.json.simple.parser.ParseException {
		// Lendo arquivo e desserializando o JSON para objeto
		try (FileReader fileReader = new FileReader("pessoa.json")) {
			JSONParser jsonParser = new JSONParser();
			JSONObject jsonObject = (JSONObject) jsonParser.parse(fileReader);

			// Criando um objeto Person a partir do JSON
			String nome = (String) jsonObject.get("Nome");
			long idade = (long) jsonObject.get("Idade");
			Pessoa deserializedPerson = new Pessoa(nome, (int) idade);

			System.out.println("Objeto desserializado: " + deserializedPerson);

		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```
