# Rest
Criando Rest




Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.

Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.

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
