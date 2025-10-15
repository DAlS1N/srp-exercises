Exercício 1 — Sistema de Notas Escolares
Violações de SRP

A classe Boletim realiza múltiplas tarefas:

Gerencia notas e calcula média

Gera situação (Aprovado/Reprovado)

Salva boletim em arquivo

Envia boletim por e-mail

Refatoração
// Classe principal
public class Boletim {
    private String nomeAluno;
    private List<Double> notas = new ArrayList<>();

    public Boletim(String nomeAluno) {
        this.nomeAluno = nomeAluno;
    }

    public void adicionarNota(double nota) {
        notas.add(nota);
    }

    public double calcularMedia() {
        return notas.stream().mapToDouble(Double::doubleValue).average().orElse(0);
    }

    public String getNomeAluno() {
        return nomeAluno;
    }
}

// Classe responsável pela avaliação
public class Avaliador {
    public String gerarSituacao(Boletim boletim) {
        double media = boletim.calcularMedia();
        return media >= 7 ? "Aprovado" : "Reprovado";
    }
}

// Classe responsável por salvar o boletim
public class BoletimFileSaver {
    public void salvar(Boletim boletim) throws IOException {
        try (FileWriter writer = new FileWriter(boletim.getNomeAluno() + "_boletim.txt")) {
            writer.write("Aluno: " + boletim.getNomeAluno() + "\nMédia: " + boletim.calcularMedia() + "\n");
        }
    }
}

// Classe responsável por enviar e-mail
public class BoletimEmailService {
    public void enviar(String email, Boletim boletim) {
        System.out.println("Enviando boletim do aluno " + boletim.getNomeAluno() + " para " + email);
    }
}

Benefícios

Alterações em uma parte do sistema não impactam outras.

Maior clareza e facilidade de manutenção.

Classes reutilizáveis e testáveis.
