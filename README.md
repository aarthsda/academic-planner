# Planejador Acadêmico

Sistema de apoio à decisão para planejamento curricular universitário, com análise de pré-requisitos, acompanhamento do progresso acadêmico e sugestões de disciplinas compatíveis com a oferta semestral e os horários disponíveis.

> **Status do projeto:** fase de análise e modelagem. A implementação ainda não começou; os requisitos e o modelo de dados serão validados com a matriz curricular real e, quando disponível, com os arquivos de oferta de disciplinas e horários.

## Visão geral

O Planejador Acadêmico pretende ajudar estudantes a compreender sua trajetória curricular e tomar decisões mais informadas sobre quais disciplinas cursar em cada período letivo.

A partir da matriz curricular, das regras de pré-requisitos, do histórico acadêmico informado pelo estudante e das ofertas semestrais, o sistema deverá:

- Exibir um mapa curricular com o estado das disciplinas.
- Identificar disciplinas liberadas, bloqueadas ou com situação indeterminada.
- Explicar os motivos dos bloqueios, incluindo cadeias de dependências.
- Cruzar disciplinas elegíveis com as ofertas de um semestre específico.
- Avaliar turmas e horários para detectar conflitos.
- Sugerir combinações de disciplinas compatíveis com a grade do estudante.
- Permitir simulações hipotéticas sem alterar o histórico acadêmico real.

O projeto começa com foco em um curso universitário e utiliza arquivos importados como fonte de dados. A integração direta com sistemas institucionais não faz parte do escopo inicial.

## Problema que o projeto busca resolver

Matrizes curriculares, pré-requisitos, históricos e horários de oferta podem estar distribuídos em diferentes documentos. Isso dificulta identificar as consequências de uma reprovação, entender por que uma disciplina está bloqueada e descobrir quais opções podem ser cursadas no próximo semestre.

O sistema busca reunir essas informações em uma visão organizada, explicável e útil para o planejamento acadêmico.

## Objetivos

- Representar a matriz curricular e suas versões.
- Modelar pré-requisitos, correquisitos, alternativas e exceções documentadas.
- Acompanhar o progresso acadêmico informado pelo estudante.
- Explicar dependências e bloqueios curriculares.
- Importar e validar ofertas semestrais e horários.
- Detectar conflitos de horário por turma.
- Recomendar opções com base em regras explícitas e dados disponíveis.
- Manter a integridade, a rastreabilidade e a explicabilidade dos resultados.

## Funcionalidades planejadas

### Núcleo do MVP

- [ ] Importação da matriz curricular.
- [ ] Validação dos dados antes da confirmação da importação.
- [ ] Representação de pré-requisitos simples, cumulativos e alternativos.
- [ ] Registro do histórico acadêmico.
- [ ] Mapa curricular com estados das disciplinas.
- [ ] Explicação de bloqueios diretos e indiretos.
- [ ] Importação de ofertas, turmas e horários.
- [ ] Cálculo de elegibilidade separado da disponibilidade no semestre.
- [ ] Detecção de conflitos de horário.
- [ ] Sugestões de disciplinas e turmas compatíveis.
- [ ] Simulação de cenários acadêmicos sem modificar o histórico real.

### Possíveis evoluções

- [ ] Salvamento de planos acadêmicos.
- [ ] Exportação de planos para PDF ou CSV.
- [ ] Comparação entre cenários de planejamento.
- [ ] Suporte a mais cursos e versões curriculares.
- [ ] Integração institucional, caso exista uma fonte autorizada e viável.
- [ ] Técnicas de otimização para selecionar combinações de disciplinas segundo critérios configuráveis.

As funcionalidades acima são planejadas; sua presença nesta lista não significa que já estejam implementadas.

## Princípios de funcionamento

### 1. Elegibilidade não é o mesmo que oferta

O sistema deverá avaliar separadamente:

1. **Elegibilidade acadêmica:** o estudante atende aos pré-requisitos?
2. **Oferta no período:** a disciplina foi ofertada no semestre selecionado?
3. **Compatibilidade de horário:** existe uma turma cujos horários não conflitam com os compromissos selecionados?
4. **Disponibilidade de vagas:** existe informação confiável sobre vagas? Se não houver, o sistema não deverá presumir que uma vaga está garantida.

Uma disciplina pode ser elegível, mas não ofertada; pode ser ofertada, mas não ter turma compatível; ou pode ter uma turma compatível sem que a disponibilidade de vagas seja conhecida.

### 2. Resultados explicáveis

As recomendações deverão apresentar os motivos relevantes: requisitos satisfeitos, pendências, conflitos e limitações dos dados. O motor de regras deverá ser determinístico e testável, em vez de depender de decisões opacas de uma IA generativa.

### 3. Incerteza explícita

Quando os dados forem ausentes, inconsistentes ou ambíguos, o sistema deverá sinalizar a situação como indeterminada ou solicitar correção. Não deverá inventar regras para preencher lacunas.

### 4. Dados versionados e importações seguras

Matrizes curriculares e ofertas devem ser rastreáveis por versão. Arquivos importados deverão passar por validação e pré-visualização antes de serem publicados. Uma importação inválida não deverá deixar os dados ativos parcialmente atualizados.

## Limites e escopo

- O sistema não substitui a matrícula oficial nem o histórico acadêmico institucional.
- As recomendações são ferramentas de apoio e não decisões institucionais vinculantes.
- O sistema não garante a conclusão do curso em uma data específica.
- O período sugerido na matriz não será tratado automaticamente como impedimento de matrícula.
- Pré-requisitos, correquisitos, equivalências e exceções só serão aplicados conforme dados documentados ou regras confirmadas.
- A ausência de informação sobre vagas não significa que existam vagas disponíveis.
- Simulações não devem modificar o histórico real do estudante.
- A primeira versão utilizará dados importados; integração institucional fica fora do escopo inicial.

## Arquitetura proposta

A arquitetura inicial recomendada é um **monólito modular**, com frontend, backend/API e banco de dados relacional. Microserviços e infraestrutura distribuída não são prioridades antes da validação do domínio.

Módulos conceituais previstos:

- **Importação e validação:** leitura, normalização, pré-visualização e publicação dos arquivos.
- **Catálogo curricular:** cursos, versões curriculares, disciplinas e componentes.
- **Motor de regras acadêmicas:** pré-requisitos, correquisitos, alternativas e explicações de bloqueio.
- **Histórico acadêmico:** tentativas e estados informados pelo estudante.
- **Ofertas e turmas:** períodos letivos, ofertas, seções e encontros de aula.
- **Planejador:** cruzamento de elegibilidade, oferta e compatibilidade de horários.
- **Simulador:** avaliação de cenários hipotéticos isolados.
- **Interface:** mapa curricular, filtros, resultados e explicações.

O cálculo central da elegibilidade deverá ficar no backend, evitando regras duplicadas e divergentes na interface.

## Tecnologias

A stack ainda **não foi definida**. As opções em avaliação são:

| Opção | Frontend | Backend | Banco de dados | Considerações |
|---|---|---|---|---|
| A | React + TypeScript | Node.js + TypeScript | PostgreSQL | Compartilha a linguagem principal entre frontend e backend. |
| B | React + TypeScript | PHP | MySQL | Aproveita experiência com PHP, PDO, MySQL e XAMPP. |
| C | React + TypeScript | FastAPI / Python | PostgreSQL | Pode ser útil para futuras análises de dados e otimização. |

A decisão será tomada após avaliar os objetivos de aprendizagem, a experiência disponível e as necessidades identificadas nos arquivos reais. Nenhuma dessas tecnologias deve ser interpretada como já adotada.

## Modelo de dados conceitual

As principais entidades previstas são:

- **Curso**
- **Versão curricular**
- **Disciplina**
- **Componente curricular**
- **Regra de requisito**
- **Estudante**
- **Histórico acadêmico**
- **Período letivo**
- **Oferta**
- **Turma ou seção**
- **Encontro de horário**
- **Lote de importação e versão dos dados**
- **Plano acadêmico**
- **Cenário de simulação**
- **Item do plano**

A estrutura definitiva será validada com a matriz curricular real. Disciplina e turma serão entidades distintas; histórico e plano acadêmico também serão separados. A identidade das disciplinas deverá priorizar códigos estáveis em vez de depender apenas dos nomes.

## Validação e testes planejados

O sistema deverá ser testado, no mínimo, para os seguintes cenários:

- Um pré-requisito aprovado libera a disciplina dependente, quando não há outras pendências.
- Um pré-requisito reprovado não é considerado satisfeito.
- Uma cadeia de dependências permite identificar bloqueios indiretos.
- Uma regra alternativa do tipo `A OU B` é satisfeita quando uma alternativa válida é cumprida.
- Uma disciplina elegível, mas não ofertada, é identificada corretamente.
- Uma disciplina ofertada sem turmas compatíveis é diferenciada de uma disciplina bloqueada.
- Turmas diferentes são avaliadas individualmente quanto a conflitos.
- Códigos de pré-requisitos inexistentes são sinalizados na importação.
- Alterações curriculares preservam a identificação das versões anteriores.
- Uma simulação não altera o histórico acadêmico real.
- Dados duplicados, horários inválidos, requisitos circulares e informações incompletas são detectados ou sinalizados.

## Estrutura documental sugerida

```text
academic-planner/
├── README.md
├── docs/
│   ├── requisitos.md
│   ├── regras-de-negocio.md
│   ├── modelo-conceitual.md
│   ├── modelo-logico.md
│   ├── arquitetura.md
│   └── decisoes-tecnicas.md
├── data-samples/
│   └── README.md
├── src/
└── tests/
```

Essa é uma estrutura sugerida para a evolução do repositório. Os diretórios de implementação serão criados quando a fase de desenvolvimento começar. Exemplos de dados devem ser fictícios ou devidamente anonimizados e não devem conter informações pessoais desnecessárias.

## Próximas etapas

1. Analisar a matriz curricular real.
2. Analisar, se disponível, o documento de pré-requisitos e a oferta semestral com horários.
3. Identificar campos, regras, exceções, inconsistências e ambiguidades.
4. Propor o modelo lógico de dados e o formato de importação.
5. Confirmar as decisões de arquitetura e a stack.
6. Definir casos de teste com base nas regras documentadas.
7. Implementar o MVP de forma incremental.

## Estado atual

**Fase:** análise e modelagem conceitual.

**Próxima entrega:** validação do modelo com os arquivos reais da matriz curricular e das ofertas.

**Implementação:** ainda não iniciada.

## Contribuições e decisões

Neste estágio, o foco está em validar o problema, as regras acadêmicas e o modelo de dados antes de escrever código. Decisões importantes devem ser documentadas, e qualquer regra não confirmada deve permanecer identificada como pendente.

## Licença

A licença ainda não foi definida. Escolha uma licença antes de permitir formalmente reutilização, modificação ou distribuição do código.
