Classe Aluno
```java
import java.util.ArrayList;

public class Aluno {
    String cpf;
    String nome;
    String email;
    ArrayList<Curso> cursos;
    ArrayList<Turma> turmas;

    public Aluno(String cpf, String nome, String email) {
        
    }
}
```

Classe Disciplina
```java
public class Disciplina {
    String descricao;

    public Disciplina(String descricao) {
        this.descricao = descricao;
    }

    
    
}

```

Classe Turma
```java
import java.util.ArrayList;

public class Turma {
    String descricao;
    Curso curso;
    Disciplina disciplina;
    ArrayList<Professor> professores;
    ArrayList<Aluno> alunos;
    
    public Turma(String descricao, Disciplina disciplina) {
        this.descricao = descricao;
        this.disciplina = disciplina;
    }

    
}

```

Classe Professor
```java
import java.util.ArrayList;

public class Professor {
    String cpf;
    String nome;
    String email;
    Curso curso;
    ArrayList<Turma> turmas;

    public Professor(String cpf, String nome, String email, Curso curso) {
        this.cpf = cpf;
        this.nome = nome;
        this.email = email;
        this.curso = curso;
    }
}

```

Classe Curso
```java
import java.util.ArrayList;

public class Curso {
    String descricao;
    String sigla;
    Area area;
    ArrayList<Aluno> alunos;

    public Curso(String descricao, String sigla, Area area) {
        this.descricao = descricao;
        this.sigla = sigla;
        this.area = area;
    }
    
}

```

Classe Area
```java
public class Area {
    
}
```
