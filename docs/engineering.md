# Decisões de engenharia

[← Voltar ao case](../README.md)

## Contexto de organização no acesso aos dados

**Problema:** a mesma pessoa pode trabalhar em várias empresas. Uma tela correta não basta se a API conseguir consultar um recurso de outra organização.

**Abordagem:** associar o contexto de organização à autenticação, propagá-lo durante a requisição e restringir consultas e operações. A autorização considera o papel da pessoa e o recurso solicitado.

**Validação:** o projeto contém uma suíte de isolamento com PostgreSQL temporário via Testcontainers. Ela permite verificar o comportamento junto da persistência.

**Limite:** isolamento na aplicação exige disciplina em novos caminhos de acesso. Não se deve afirmar que existe Row-Level Security no banco apenas por aparecer em um documento de planejamento.

## Sessão e requisições simultâneas

**Problema:** várias requisições podem receber uma resposta de sessão expirada ao mesmo tempo. Se cada uma tentar renovar os tokens, surgem disputas e estados inconsistentes.

**Abordagem:** centralizar a renovação no cliente, coordenar as requisições concorrentes e manter o estado de sessão como referência para a navegação. O backend utiliza tokens de acesso e renovação com rotação.

**Consequência:** o comportamento precisa ser considerado em conjunto com armazenamento e ciclo de vida de cada plataforma.

## Eventos em tempo real

**Problema:** alterações em tarefas e conversas precisam chegar às pessoas autorizadas sem exigir atualização manual constante.

**Abordagem:** WebSocket complementa a API HTTP, com entrega de eventos no contexto da organização. A interface integra esses eventos ao estado local.

**Limite:** entrega em uma instância não comprova distribuição entre várias instâncias. Escala horizontal exige coordenação explícita de eventos e testes de reconexão e recuperação.

## Anexos e visibilidade

**Problema:** uma imagem de marca pode ser pública, enquanto um arquivo de conversa deve acompanhar as permissões do conteúdo.

**Abordagem:** separar os usos de armazenamento e utilizar acesso temporário para anexos privados. A apresentação do arquivo também precisa funcionar no navegador e nos aplicativos nativos.

**Consequência:** permissões do armazenamento, validade do acesso e cache fazem parte do comportamento do produto.

## Uma base Flutter em vários ambientes

**Problema:** compartilhar interface não elimina diferenças de navegação, teclado, notificações e integrações nativas.

**Abordagem:** manter os fluxos comuns na base Flutter e isolar comportamentos específicos. O CI inclui build web porque análise e testes em ambiente nativo não garantem que toda dependência funcione no navegador.

## O que este material permite avaliar

O case apresenta a amplitude do trabalho, a divisão de responsabilidades e decisões que atravessam frontend, backend e operação. O produto publicado permite conhecer sua entrada pública. Uma avaliação do código proprietário ou dos fluxos autenticados exige acesso apropriado ao projeto.

Este documento não é certificação de segurança ou conformidade jurídica. As métricas comerciais e operacionais não fazem parte do material público.
