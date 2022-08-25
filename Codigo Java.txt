package consultapersonalizada;
import javax.swing.JOptionPane; //Lança  caixa de diálogo para o usuário digitar o que desejar
import java.sql.Connection; //Faz a conexão com o Banco de Dados
import java.sql.Statement; // Permite o uso da Linguagem SQL
import java.sql.DriverManager; // Responsável por disponibizar o acesso do Banco de Dados
import java.sql.ResultSet; // Trás as informações buscadas
import java.sql.ResultSetMetaData; // Faz as gerenciações das informações buscadas
import java.sql.PreparedStatement; // Prepara consulta personalizada
import java.sql.SQLException; // Acusa os erros ortográficos e etc

public class ConsultaPersonalizada {
    
    static final String banco = "jdbc:mysql://localhost:3306/concessionária"; // Guarda  o endereço do Banco de Dados
    public static void main(String[] args) {
        Connection conexao = null; // Se conecta com o Banco de Dados
        PreparedStatement consulta = null; // Consultar o Banco de Dados selecionado
        ResultSet resultados = null; // Exibir o resultado do Banco de Dados
        try {
            
            conexao = DriverManager.getConnection(banco,"root",""); // Conexão do Banco de Dados com usuário e senha
            consulta = conexao.prepareStatement("select * from veiculo where fabricante = ? "); // Seleciona onde será feita a consulta
            String fabricante = JOptionPane.showInputDialog(null,"Informe o fabricante a ser consultado"); // Mensagem a ser exibida para o usuário na caixa de texto
            consulta.setString(1,fabricante); // Realiza uma consulta através do fabricante indicado
            resultados = consulta.executeQuery(); // Exibi resultados atráves do foi consultado anteriormente
            ResultSetMetaData colunas = resultados.getMetaData(); // Permite obter inforações sobre a tabela com o resultado da consulta
            
            int numeroColunas = colunas.getColumnCount(); // Retorna o número de colunas referente ao que foi consultado
            System.out.println("informações do Banco de dados"); // Imprime está mensagem para o usuário
            
            while (resultados.next()){ // Enquanto estiver entrando dados, ele irá exibi-los
                for (int i=1; i <= numeroColunas; i++ ) // Conta o número de colunas
                System.out.println("dados " + resultados.getObject(i)); // imprime o resultado da consulta
                System.out.println();
            }
        }
        catch (SQLException erro){ // Irá capturar um erro caso haja
            erro.printStackTrace(); // Irá imprimir o erro que está sendo cometido
        }
        finally {
            try {
                resultados.close(); // Fecha resultado
                consulta.close(); //Fecha consulta
                conexao.close(); // Fecha conexão
            }
            catch (Exception erronovo) { // Captura um erro novo caso haja
                erronovo.printStackTrace(); // Irá imprimir o novo erro que está sendo cometido
            }
        }
    }
}