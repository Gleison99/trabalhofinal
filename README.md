# trabalhofinal
Trabalho sobre doação prof. Montanha

1. Proposta de Problema
Desenvolver um sistema para gerenciar doações às vítimas das enchentes no Sul do Brasil, permitindo registrar, calcular e armazenar informações sobre as doações.

2. Requisitos Funcionais
Utilizaremos uma IA Generativa para identificar os requisitos funcionais do sistema: OpenAI ChatGPT

Cadastro de Doações: o sistema deve permitir o registro de novas doações, incluindo tipo (dinheiro, alimentos, roupas), quantidade e data.
Cálculo Total de Doações: o sistema deve calcular e apresentar o total de doações recebidas.
Armazenamento em Arquivo: todas as informações sobre as doações devem ser armazenadas em um arquivo texto para posterior consulta e análise.

3. Crítica à IA

Falta de Contexto Específico: A IA pode gerar requisitos que são genéricos e não totalmente alinhados com o contexto específico do projeto. Por exemplo, na proposta inicial, a IA sugeriu um sistema de gerenciamento de doações, que não está diretamente relacionado ao desafio proposto pelo instituto de pesquisa.

Necessidade de Revisão Humana: As sugestões da IA necessitam de revisão e refinamento por parte de especialistas humanos para garantir que os requisitos sejam precisos e relevantes. A IA pode oferecer uma boa base, mas a intervenção humana é essencial para adaptar os requisitos ao contexto específico da pesquisa científica.

Limitações em Detalhamento Técnico: Embora a IA possa identificar requisitos funcionais de alto nível, ela pode não fornecer o nível de detalhe técnico necessário para a implementação direta. Por exemplo, a IA sugeriu funcionalidades como "Cadastro de Doações" e "Cálculo Total de Doações", mas os detalhes de implementação e integração com outros sistemas precisariam ser elaborados por desenvolvedores experientes.

Consideração de Requisitos Não Funcionais: A IA pode não abordar completamente os requisitos não funcionais, como desempenho, segurança e escalabilidade, que são críticos em um sistema de gestão de dados científicos. Estes requisitos geralmente requerem uma análise mais aprofundada e específica que pode não ser completamente coberta pelas sugestões iniciais da IA.

Criatividade Limitada: A IA pode sugerir soluções baseadas em padrões comuns e conhecidos, mas pode não ter a mesma capacidade de inovação e criatividade que um ser humano pode trazer para resolver problemas específicos de maneira mais eficiente ou inovadora.

4. Diagrama de Classes
Descrição textual do diagrama de classes:

Classe Donation

Atributos: tipo (String), quantidade (int), data (String)
Métodos: getters e setters para todos os atributos
Classe DonationManager

Atributos: lista de doações (ArrayList<Donation>)
Métodos:
addDonation(Donation donation): adiciona uma nova doação à lista
calculateTotalDonations(): calcula o total de todas as doações registradas
saveToFile(String filename): salva as informações das doações em um arquivo texto
loadFromFile(String filename): carrega as informações das doações de um arquivo texto
5. Estratégia de Programação com IA
Exploraremos estratégias como revisão de código, geração de exemplos e prática de exercícios com a assistência da IA.

6. Codificação do Programa
Aqui está o código Java que implementa o sistema de gerenciamento de doações:

java
Copiar código
import java.io.*;
import java.util.*;

// Classe para representar uma doação
class Donation {
    private String type;
    private int quantity;
    private String date;

    public Donation(String type, int quantity, String date) {
        this.type = type;
        this.quantity = quantity;
        this.date = date;
    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }
}

// Classe para gerenciar as doações
class DonationManager {
    private ArrayList<Donation> donations;

    public DonationManager() {
        this.donations = new ArrayList<>();
    }

    public void addDonation(Donation donation) {
        donations.add(donation);
    }

    public int calculateTotalDonations() {
        int total = 0;
        for (Donation donation : donations) {
            total += donation.getQuantity();
        }
        return total;
    }

    public void saveToFile(String filename) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            for (Donation donation : donations) {
                writer.printf("%s;%d;%s\n", donation.getType(), donation.getQuantity(), donation.getDate());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void loadFromFile(String filename) {
        donations.clear();
        try (Scanner scanner = new Scanner(new File(filename))) {
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                String[] parts = line.split(";");
                if (parts.length == 3) {
                    String type = parts[0];
                    int quantity = Integer.parseInt(parts[1]);
                    String date = parts[2];
                    Donation donation = new Donation(type, quantity, date);
                    donations.add(donation);
                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}

// Classe principal para teste do sistema
public class DonationSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        DonationManager donationManager = new DonationManager();

        while (true) {
            System.out.println("\nMENU:");
            System.out.println("1. Registrar nova doação");
            System.out.println("2. Calcular total de doações");
            System.out.println("3. Salvar doações em arquivo");
            System.out.println("4. Carregar doações de arquivo");
            System.out.println("5. Sair");
            System.out.print("Escolha uma opção: ");
            int option = scanner.nextInt();
            scanner.nextLine(); // Consumir a quebra de linha após o nextInt()

            switch (option) {
                case 1:
                    System.out.print("Tipo de doação (dinheiro, alimentos, roupas, etc.): ");
                    String type = scanner.nextLine();
                    System.out.print("Quantidade: ");
                    int quantity = scanner.nextInt();
                    scanner.nextLine(); // Consumir a quebra de linha após o nextInt()
                    System.out.print("Data da doação (DD/MM/AAAA): ");
                    String date = scanner.nextLine();
                    Donation newDonation = new Donation(type, quantity, date);
                    donationManager.addDonation(newDonation);
                    System.out.println("Doação registrada com sucesso!");
                    break;
                case 2:
                    int totalDonations = donationManager.calculateTotalDonations();
                    System.out.println("Total de doações recebidas: " + totalDonations);
                    break;
                case 3:
                    System.out.print("Nome do arquivo para salvar as doações: ");
                    String saveFilename = scanner.nextLine();
                    donationManager.saveToFile(saveFilename);
                    System.out.println("Doações salvas em arquivo com sucesso!");
                    break;
                case 4:
                    System.out.print("Nome do arquivo para carregar as doações: ");
                    String loadFilename = scanner.nextLine();
                    donationManager.loadFromFile(loadFilename);
                    System.out.println("Doações carregadas do arquivo com sucesso!");
                    break;
                case 5:
                    System.out.println("Saindo do programa...");
                    System.exit(0);
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }
}
Conclusão
Este trabalho prático em Java desenvolve um sistema completo de gerenciamento de doações para ajudar as vítimas das enchentes no Sul do Brasil. Utilizamos uma IA Generativa para identificar os requisitos funcionais, implementamos o sistema conforme o diagrama de classes proposto e exploramos estratégias de programação modernas, como manipulação de arquivos e interface de usuário simples via console. Este projeto não apenas aplica os conhecimentos da disciplina, mas também demonstra a aplicação prática em um cenário de impacto social significativo.
