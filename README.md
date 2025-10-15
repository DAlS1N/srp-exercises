# 🧩 Exercício 1 — Sistema de Notas Escolares
## 🔍 Violações do SRP

A classe Boletim faz muitas coisas ao mesmo tempo:

Gerencia dados do aluno e notas.

Calcula média e situação (lógica de negócio).

Salva boletim em arquivo (persistência).

Envia e-mail (comunicação).

## ✅ Refatoração
// Responsável apenas por representar e calcular dados do aluno
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

// Responsável por gerar a situação do aluno
public class AvaliadorBoletim {
    public String gerarSituacao(Boletim boletim) {
        return boletim.calcularMedia() >= 7 ? "Aprovado" : "Reprovado";
    }
}

// Responsável por salvar em arquivo
public class BoletimArquivo {
    public void salvar(Boletim boletim) throws IOException {
        try (FileWriter writer = new FileWriter(boletim.getNomeAluno() + "_boletim.txt")) {
            writer.write("Aluno: " + boletim.getNomeAluno() + "\nMédia: " + boletim.calcularMedia() + "\n");
        }
    }
}

// Responsável por enviar e-mails
public class EmailService {
    public void enviarEmail(String email, Boletim boletim) {
        System.out.println("Enviando boletim de " + boletim.getNomeAluno() + " para " + email);
    }
}

## 💡 Benefícios da refatoração

Cada classe tem uma única responsabilidade.

Facilita testes unitários (ex: testar cálculo sem mexer com e-mail).

Permite reuso (ex: EmailService pode ser usado em outros módulos).

Modificações (como mudar o formato do arquivo) não quebram o resto do sistema.

---

# 🕒 Exercício 2 — Sistema de Relógio Digital
## 🔍 Violações do SRP

A classe RelogioDigital:

Exibe a hora.

Salva a hora em arquivo.

Toca alarme.
➡️ São 3 responsabilidades distintas.

## ✅ Refatoração
public class ExibidorHora {
    public void mostrarHora() {
        System.out.println(LocalTime.now());
    }
}

public class GravadorHora {
    public void salvarHoraEmArquivo() {
        try (FileWriter f = new FileWriter("hora.txt")) {
            f.write(LocalTime.now().toString());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

public class Alarme {
    public void tocar() {
        System.out.println("BEEP! Alarme tocando!");
    }
}

// Serviço orquestrador
public class RelogioService {
    private final ExibidorHora exibidor = new ExibidorHora();
    private final GravadorHora gravador = new GravadorHora();
    private final Alarme alarme = new Alarme();

    public void executar() {
        exibidor.mostrarHora();
        gravador.salvarHoraEmArquivo();
        alarme.tocar();
    }
}

---

# 💾 Exercício 3 — Sistema de Backup Automático
## 🔍 Responsabilidades distintas

Copiar arquivos.

Compactar arquivos.

Enviar notificação.

Registrar logs.

## ✅ Refatoração
public class CopiadorArquivos {
    public void copiar(String origem, String destino) {
        System.out.println("Copiando de " + origem + " para " + destino);
    }
}

public class Compactador {
    public void compactar(String destinoZip) {
        System.out.println("Compactando arquivos em " + destinoZip);
    }
}

public class NotificadorEmail {
    public void enviarEmail(String destinatario) {
        System.out.println("Enviando notificação para " + destinatario);
    }
}

public class GerenciadorLog {
    public void registrar(String mensagem) {
        System.out.println("LOG: " + mensagem);
    }
}

## 💡 Benefícios

Cada classe pode ser reutilizada em outros projetos (ex: Compactador num sistema de upload).

Facilita manutenção: alterar o método de notificação não afeta o backup.

Testes independentes e mais confiáveis.

---

# 💬 Exercício 4 — App de Mensagens Instantâneas
## 🔍 Responsabilidades

Envio de mensagem.

Exibição de histórico.

Salvamento no banco de dados.

Notificação de contatos.

## ✅ Refatoração
public class EnviadorMensagem {
    public void enviar(String texto) {
        System.out.println("Mensagem enviada: " + texto);
    }
}

public class HistoricoMensagens {
    public void exibir() {
        System.out.println("Exibindo histórico de mensagens...");
    }
}

public class RepositorioMensagens {
    public void salvar(String texto) {
        System.out.println("Salvando mensagem no banco: " + texto);
    }
}

public class NotificadorContato {
    public void notificar(String contato) {
        System.out.println("Notificando contato: " + contato);
    }
}

📈 Vantagens para o time de QA

Testes unitários mais diretos e isolados.

Simulação de uma funcionalidade não exige executar toda a aplicação.

Reduz erros colaterais ao fazer manutenção.

---

# 🏦 Exercício 5 — Conta Bancária
## 🔍 Responsabilidades

Operações financeiras (sacar/depositar).

Geração de extrato.

Persistência no banco.

Envio de notificações.

## ✅ Refatoração
public class ContaBancaria {
    private double saldo;

    public void depositar(double valor) { saldo += valor; }
    public void sacar(double valor) { saldo -= valor; }
    public double getSaldo() { return saldo; }
}

public class GeradorExtrato {
    public void gerar(ContaBancaria conta) {
        System.out.println("Saldo atual: " + conta.getSaldo());
    }
}

public class RepositorioConta {
    public void salvar(ContaBancaria conta) {
        System.out.println("Salvando conta no banco de dados...");
    }
}

public class ServicoEmail {
    public void enviar(String email) {
        System.out.println("Enviando notificação para " + email);
    }
}

💡 Benefícios

Mudanças no sistema de e-mail não afetam a lógica bancária.

Cada módulo pode evoluir separadamente.

Aumenta testabilidade e reuso em outros contextos (ex: reusar ServicoEmail para outros avisos).

---



# 🧠 Princípio da Responsabilidade Única (SRP)

Este repositório contém exemplos de refatoração aplicando o **Single Responsibility Principle**, parte dos princípios **SOLID**.

---

## 📘 Exercício 1 — Sistema de Notas Escolares
**Problema:** A classe `Boletim` fazia cálculo, persistência e envio de e-mail.  
**Solução:** Criadas classes `Boletim`, `AvaliadorBoletim`, `BoletimArquivo` e `EmailService`.

---

## 🕒 Exercício 2 — Sistema de Relógio Digital
**Antes:** `RelogioDigital` mostrava hora, salvava em arquivo e tocava alarme.  
**Depois:** Dividido em `ExibidorHora`, `GravadorHora`, `Alarme` e `RelogioService`.

---

## 💾 Exercício 3 — Sistema de Backup Automático
**Responsabilidades:** Copiar, compactar, notificar e registrar log.  
**Classes criadas:** `CopiadorArquivos`, `Compactador`, `NotificadorEmail`, `GerenciadorLog`.

---

## 💬 Exercício 4 — App de Mensagens Instantâneas
**Separação:** `EnviadorMensagem`, `HistoricoMensagens`, `RepositorioMensagens`, `NotificadorContato`.

---

## 🏦 Exercício 5 — Conta Bancária
**Antes:** Uma classe fazia tudo.  
**Depois:**  
- `ContaBancaria` → Operações.  
- `GeradorExtrato` → Impressão de saldo.  
- `RepositorioConta` → Persistência.  
- `ServicoEmail` → Notificações.

---

## ⚙️ Benefícios Gerais
- Código mais **modular e testável**.  
- **Facilidade de manutenção**.  
- **Alta coesão** e **baixo acoplamento**.  
- Reuso de componentes em outros sistemas.

---
