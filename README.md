#Exercício 1 — Sistema de Notas Escolares
Violações de SRP:

A classe Boletim possui várias responsabilidades:

Gerenciar notas e calcular média.

Gerar a situação do aluno (“Aprovado” ou “Reprovado”).

Salvar boletim em arquivo.

Enviar boletim por e-mail.

Refatoração:
// Responsável por armazenar notas e calcular média
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

// Responsável por gerar situação
public class Avaliador {
    public String gerarSituacao(Boletim boletim) {
        double media = boletim.calcularMedia();
        return media >= 7 ? "Aprovado" : "Reprovado";
    }
}

// Responsável por salvar boletim em arquivo
public class BoletimFileSaver {
    public void salvar(Boletim boletim) throws IOException {
        try (FileWriter writer = new FileWriter(boletim.getNomeAluno() + "_boletim.txt")) {
            writer.write("Aluno: " + boletim.getNomeAluno() + "\nMédia: " + boletim.calcularMedia() + "\n");
        }
    }
}

// Responsável por enviar boletim por e-mail
public class BoletimEmailService {
    public void enviar(String email, Boletim boletim) {
        System.out.println("Enviando boletim do aluno " + boletim.getNomeAluno() + " para " + email);
    }
}

Benefícios:

Alterações em uma parte do código não afetam as outras.

Maior facilidade de manutenção e teste.

Reutilização das classes em outros contextos.
