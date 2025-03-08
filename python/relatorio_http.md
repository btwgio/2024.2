# Análise do Servidor HTTP com e sem Threads

## 1. Experimento 1

### Execução do Servidor HTTP sem Thread

#### Testes e Análises:

1. **Apenas 1 cliente**
   - O servidor atende a requisição normalmente.
   - A resposta é enviada sem atraso perceptível.
   - O cliente recebe a página HTML fixa corretamente.

2. **2 clientes simultâneos**
   - O primeiro cliente é atendido imediatamente.
   - O segundo cliente precisa aguardar a finalização do primeiro antes de ser atendido.
   - Há um pequeno atraso para o segundo cliente.

3. **5 clientes simultâneos**
   - O servidor processa as requisições uma por vez.
   - Os clientes precisam aguardar na fila de atendimento.
   - A latência para os últimos clientes aumenta consideravelmente.

4. **10 clientes simultâneos**
   - A espera para os últimos clientes na fila se torna significativa.
   - O servidor continua processando requisições uma por vez.
   - O tempo de resposta aumenta drasticamente.

### Execução do Servidor HTTP com Thread

#### Testes e Análises:

1. **Apenas 1 cliente**
   - Semelhante ao servidor sem thread, a requisição é atendida rapidamente.
   - O cliente recebe a resposta sem atraso perceptível.

2. **2 clientes simultâneos**
   - Ambos os clientes são atendidos quase ao mesmo tempo, pois cada requisição é processada em uma thread separada.
   - O tempo de resposta para o segundo cliente é praticamente o mesmo que para o primeiro.

3. **5 clientes simultâneos**
   - O servidor consegue processar múltiplas requisições simultaneamente.
   - O tempo de resposta para todos os clientes é bem menor do que no servidor sem thread.

4. **10 clientes simultâneos**
   - O servidor consegue lidar bem com a carga maior.
   - Cada requisição é processada em uma thread separada, evitando filas longas.
   - A experiência do usuário é muito melhor comparada ao servidor sem thread.

## 2. Comparação entre Servidor com e sem Threads

| Número de Clientes | Servidor Sem Thread | Servidor Com Thread |
|-------------------|--------------------|--------------------|
| 1                | Atendimento rápido  | Atendimento rápido  |
| 2                | Atraso no segundo cliente | Ambos atendidos simultaneamente |
| 5                | Espera considerável para os últimos clientes | Tempo de resposta menor |
| 10               | Tempo de espera muito alto | Processamento paralelo, tempo reduzido |

### Diferença no Funcionamento:
- **Servidor sem thread:** Processa requisições sequencialmente, causando aumento no tempo de resposta para múltiplos clientes simultâneos.
- **Servidor com thread:** Cada requisição é tratada em uma thread separada, permitindo atendimento simultâneo e reduzindo o tempo de espera.
- O uso de threads melhora significativamente a escalabilidade e a experiência do usuário em cenários com múltiplos clientes.

### Conclusão:
A implementação com threads é essencial para servidores HTTP que precisam atender múltiplos clientes simultaneamente, pois melhora a eficiência e reduz atrasos, garantindo melhor desempenho e escalabilidade.

