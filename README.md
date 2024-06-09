# Rest
Criando Rest


Passo 3: Configuração do Flyway
Crie uma pasta db/migration dentro de src/main/resources e adicione os arquivos de migração SQL. Por exemplo, V1__Create_table.sql:

SQL

CREATE TABLE pessoa (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    idade INT NOT NULL
);
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Passo 4: Criação das Entidades
Crie a entidade Pessoa:

Java

import javax.persistence.*;
import javax.validation.constraints.*;

@Entity
public class Pessoa {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank
    @Size(max = 100)
    private String nome;

    @NotBlank
    @Email
    @Size(max = 100)
    private String email;

    @NotNull
    @Min(0)
    private Integer idade;

    // Getters e Setters
}
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Passo 5: Criação do Repositório
Crie o repositório PessoaRepository:

Java

import org.springframework.data.jpa.repository.JpaRepository;

public interface PessoaRepository extends JpaRepository<Pessoa, Long> {
}
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Passo 6: Criação do Serviço
Crie o serviço PessoaService:

Java

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.stereotype.Service;

import java.util.Optional;

@Service
public class PessoaService {

    @Autowired
    private PessoaRepository pessoaRepository;

    public Page<Pessoa> listarTodos(Pageable pageable) {
        return pessoaRepository.findAll(pageable);
    }

    public Optional<Pessoa> buscarPorId(Long id) {
        return pessoaRepository.findById(id);
    }

    public Pessoa salvar(Pessoa pessoa) {
        return pessoaRepository.save(pessoa);
    }

    public void deletar(Long id) {
        pessoaRepository.deleteById(id);
    }
}
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Passo 7: Criação do Controlador
Crie o controlador PessoaController:

Java

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import java.util.Optional;

@RestController
@RequestMapping("/api/pessoas")
public class PessoaController {

    @Autowired
    private PessoaService pessoaService;

    @GetMapping
    public Page<Pessoa> listarTodos(Pageable pageable) {
        return pessoaService.listarTodos(pageable);
    }

    @GetMapping("/{id}")
    public ResponseEntity<Pessoa> buscarPorId(@PathVariable Long id) {
        Optional<Pessoa> pessoa = pessoaService.buscarPorId(id);
        return pessoa.map(ResponseEntity::ok).orElseGet(() -> ResponseEntity.notFound().build());
    }

    @PostMapping
    public ResponseEntity<Pessoa> salvar(@Valid @RequestBody Pessoa pessoa) {
        Pessoa pessoaSalva = pessoaService.salvar(pessoa);
        return ResponseEntity.status(HttpStatus.CREATED).body(pessoaSalva);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Pessoa> atualizar(@PathVariable Long id, @Valid @RequestBody Pessoa pessoa) {
        if (!pessoaService.buscarPorId(id).isPresent()) {
            return ResponseEntity.notFound().build();
        }
        pessoa.setId(id);
        Pessoa pessoaAtualizada = pessoaService.salvar(pessoa);
        return ResponseEntity.ok(pessoaAtualizada);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deletar(@PathVariable Long id) {
        if (!pessoaService.buscarPorId(id).isPresent()) {
            return ResponseEntity.notFound().build();
        }
        pessoaService.deletar(id);
        return ResponseEntity.noContent().build();
    }
}
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Passo 8: Paginação
A paginação já está configurada no método listarTodos do controlador e do serviço, utilizando o parâmetro Pageable.

Conclusão
Com esses passos, você terá uma API REST completa em Java com Spring Boot, utilizando MySQL, Flyway para migrações, Bean Validation para validações e paginação dos dados.


Response stopped

N
