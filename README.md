# **Sistema de Gestão de Eventos Acadêmicos**  

O **Sistema de Gestão de Eventos Acadêmicos** tem como principal objetivo facilitar a seleção e participação de alunos e servidores em eventos acadêmicos, como projetos de pesquisa e extensão, viagens pedagógicas, entre outras. Além disso, o sistema contará com perfis para cada usuário, permitindo um melhor gerenciamento de suas informações.  

Link para a página do sistema: https://rrs92.github.io/SistemaGestaoEventosAcademicos/FrontEnd/index.html

Link para o video de deonstração do sistema: https://youtu.be/79lH1K17pqA?si=4ETpwXYzKj99r1_9

## **Garantia de Qualidade com Testes**  

O sistema utiliza **testes unitários** e **testes de integração** para assegurar a confiabilidade, integridade e bom funcionamento. Ambos desempenham papéis complementares na validação das funcionalidades e da comunicação entre os componentes do sistema.

---

## **Testes Unitários**  

Os **testes unitários** têm o papel de validar funcionalidades específicas de forma isolada, garantindo que cada método e função se comporte como esperado. Eles foram desenvolvidos com **JUnit 5** para o backend em **Spring Boot** e cobrem:

- **Autenticação e autorização**: Validação de login, controle de acesso e permissões.  
- **Cadastro e gerenciamento de eventos**: Verificação das operações de criação, edição e exclusão de eventos acadêmicos.  
- **Gerenciamento de usuários**: Testes para criação, atualização e exclusão de perfis de alunos e servidores.  
- **Regras de negócio**: Garantia de que prazos de inscrição e requisitos sejam respeitados.  
- **Integração com o banco de dados**: Testes para verificar operações de persistência e consulta de dados.  

### **Formatação e Estrutura dos Testes Unitários**  

1. **Organização dos Testes**  
   - Localizados na pasta `src/test/java`.  
   - Divididos por pacotes que seguem a estrutura do código principal.  

2. **Convenções de Nomenclatura**  
   - **Classe de Teste**: Sufixo `Test` — ex.: `EventoServiceTest.java`.  
   - **Métodos de Teste**: Nome no padrão `metodo_esperado_comportamento()` — ex.: `cadastroEvento_Sucesso()`.  

3. **Framework e Principais Anotações**  
   - **JUnit 5** com as anotações `@Test`, `@BeforeEach`, `@AfterEach`.  
   - **Mockito** para simulação de dependências.  
   - Testes rápidos e independentes, verificando apenas o comportamento interno das classes.

---

## **Testes de Integração**  

Os **testes de integração** verificam se os diferentes módulos do sistema funcionam corretamente quando combinados, garantindo a comunicação entre camadas e serviços externos, como o banco de dados e a API REST.

### **Principais Áreas Testadas**  
- **Autenticação**: Verificação do fluxo completo, desde o envio de dados até a geração do token de acesso.  
- **Cadastro e consulta de eventos**: Validação do processo de criação, atualização e consulta no banco de dados.  
- **Comunicação entre serviços**: Teste da interação entre controladores, serviços e repositórios.  

### **Formatação e Estrutura dos Testes de Integração**  

1. **Organização**  
   - Localizados na pasta `src/test/java`, organizados em uma subpasta específica (`integration`) para separar dos unitários.  

2. **Convenções de Nomenclatura**  
   - **Classe de Teste**: Sufixo `IT` — ex.: `EventoServiceIT.java`.  
   - **Métodos de Teste**: Seguem o mesmo padrão dos unitários (`metodo_esperado_comportamento()`).  

3. **Framework e Principais Anotações**  
   - **@SpringBootTest** para inicializar o contexto do Spring.  
   - **MockMvc** para simular requisições HTTP.  
   - **@Transactional** para garantir rollback após cada teste e manter o banco de dados consistente.  

---

Esses testes juntos garantem a **confiabilidade** do sistema, reduzindo falhas em produção e facilitando a evolução do código com segurança.

**A seguir alguns exemplos de testes unitarios e de integração:**

```java
//Exemplo de teste Unitario
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class EventoServiceTest {

    @Test
    void cadastroEvento_Sucesso() {
        Evento evento = new Evento("Palestra", "2025-03-15");
        EventoService service = new EventoService();
        
        boolean resultado = service.cadastrarEvento(evento);
        
        assertTrue(resultado);
    }
}


//Exemplo teste de integração
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.web.servlet.MockMvc;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
class EventoControllerIT {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void listarEventos_Sucesso() throws Exception {
        mockMvc.perform(get("/eventos"))
               .andExpect(status().isOk());
    }
}

  
