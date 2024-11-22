No IntelliJ IDEA, a estrutura básica do projeto será criada, com pastas como src/main/java e src/main/resources.

Você verá uma pasta src/main/java/com/suporte/operacional onde você criará as classes relacionadas ao feedback.

3. Criando o Modelo (Entidade) de Feedback:
Dentro do diretório do pacote com.suporte.operacional.model, crie uma classe chamada Feedback.

package com.suporte.operacional.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.time.LocalDateTime;

@Entity
public class Feedback {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private int avaliacao;
    private String comentario;
    private LocalDateTime dataEnvio;
    private String origem;
    private Long idProduto;
    private boolean prioridade;

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public int getAvaliacao() {
        return avaliacao;
    }

    public void setAvaliacao(int avaliacao) {
        this.avaliacao = avaliacao;
    }

    public String getComentario() {
        return comentario;
    }

    public void setComentario(String comentario) {
        this.comentario = comentario;
    }

    public LocalDateTime getDataEnvio() {
        return dataEnvio;
    }

    public void setDataEnvio(LocalDateTime dataEnvio) {
        this.dataEnvio = dataEnvio;
    }

    public String getOrigem() {
        return origem;
    }

    public void setOrigem(String origem) {
        this.origem = origem;
    }

    public Long getIdProduto() {
        return idProduto;
    }

    public void setIdProduto(Long idProduto) {
        this.idProduto = idProduto;
    }

    public boolean isPrioridade() {
        return prioridade;
    }

    public void setPrioridade(boolean prioridade) {
        this.prioridade = prioridade;
    }
}
A classe Feedback tem os campos que são necessários (avaliação, comentário, dataEnvio, origem, idProduto, e prioridade).
A anotação @Entity marca a classe como uma entidade JPA, ou seja, ela será mapeada para uma tabela no banco de dados.
4. Criando o Repositório:
Agora, você criará um repositório para a classe Feedback. Dentro de com.suporte.operacional.repository, crie a interface FeedbackRepository.

package com.suporte.operacional.repository;

import com.suporte.operacional.model.Feedback;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface FeedbackRepository extends JpaRepository<Feedback, Long> {

    // Método para buscar feedbacks por origem
    List<Feedback> findByOrigem(String origem);
    
}
O FeedbackRepository estende JpaRepository, que oferece métodos prontos para operações de CRUD.
O método findByOrigem permite filtrar os feedbacks pela origem.
5. Criando o Serviço:
Agora, vamos criar o serviço para gerenciar os feedbacks. Dentro de com.suporte.operacional.service, crie a classe FeedbackService.

package com.suporte.operacional.service;

import com.suporte.operacional.model.Feedback;
import com.suporte.operacional.repository.FeedbackRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class FeedbackService {

    @Autowired
    private FeedbackRepository feedbackRepository;

    // Método para criar um novo feedback
    public Feedback createFeedback(Feedback feedback) {
        feedback.setDataEnvio(java.time.LocalDateTime.now());
        return feedbackRepository.save(feedback);
    }

    // Método para consultar feedbacks por origem
    public List<Feedback> getFeedbacksByOrigem(String origem) {
        return feedbackRepository.findByOrigem(origem);
    }

    // Método para marcar um feedback como prioritário
    public Feedback setFeedbackPrioritario(Long feedbackId, boolean prioridade) {
        Optional<Feedback> feedback = feedbackRepository.findById(feedbackId);
        if (feedback.isPresent()) {
            Feedback f = feedback.get();
            f.setPrioridade(prioridade);
            return feedbackRepository.save(f);
        }
        return null;
    }
}
O FeedbackService contém os métodos necessários para a criação, consulta e atualização de feedbacks.
6. Criando o Controlador (Controller):
Agora, crie o controlador em com.suporte.operacional.controller para expor as APIs REST.

package com.suporte.operacional.controller;

import com.suporte.operacional.model.Feedback;
import com.suporte.operacional.service.FeedbackService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/feedbacks")
public class FeedbackController {

    @Autowired
    private FeedbackService feedbackService;

    // Endpoint para criar um feedback
    @PostMapping
    public Feedback createFeedback(@RequestBody Feedback feedback) {
        return feedbackService.createFeedback(feedback);
    }

    // Endpoint para consultar feedbacks por origem
    @GetMapping("/origem/{origem}")
    public List<Feedback> getFeedbacksByOrigem(@PathVariable String origem) {
        return feedbackService.getFeedbacksByOrigem(origem);
    }

    // Endpoint para definir prioridade de um feedback
    @PutMapping("/{id}/prioridade")
    public Feedback setFeedbackPrioritario(@PathVariable Long id, @RequestParam boolean prioridade) {
        return feedbackService.setFeedbackPrioritario(id, prioridade);
    }
}
O FeedbackController contém os endpoints para:
POST /api/feedbacks: Criar um novo feedback.
GET /api/feedbacks/origem/{origem}: Consultar feedbacks por origem.
PUT /api/feedbacks/{id}/prioridade: Alterar o status de prioridade de um feedback.
7. Configuração do Banco de Dados:
No arquivo src/main/resources/application.properties, configure o banco de dados. Se você estiver usando o banco H2 para desenvolvimento, você pode adicionar:

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
8. Executando a Aplicação:
Para executar o projeto, basta rodar a classe principal do Spring Boot (geralmente, ela se chama SistemaSuporteApplication.java). Clique com o botão direito sobre a classe e selecione Run 'SistemaSuporteApplication'.

A API estará disponível em http://localhost:8080.
