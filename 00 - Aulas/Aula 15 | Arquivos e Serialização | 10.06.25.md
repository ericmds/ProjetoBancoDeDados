# Exemplo 1
## Classe Aluno
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

## Classe PrincipalLeitura
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

## Classe PrincipalEscritura
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

# Exemplo 2
## Classe Arquivo
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

## Classe Principal
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
