# Arquitetura de Nuvem para Aplicação de Restaurante

Este repositório contém o diagrama de arquitetura de uma aplicação web de restaurante, demonstrando uma configuração comum e escalável utilizando serviços como **Amazon EC2** e **Amazon EBS**. O objetivo é ilustrar como a aplicação pode lidar com um alto volume de tráfego, mantendo a disponibilidade e a performance.

---

### Diagrama de Arquitetura

O diagrama abaixo mostra o fluxo de dados desde o usuário até os principais componentes de backend.

**INSERIR IMAGEM DO DIAGRAMA AQUI**

Para incluir a imagem, você pode:
1.  Exportar o seu diagrama do `draw.io` como uma imagem (`.png` ou `.svg`).
2.  Salvar a imagem na pasta do seu repositório. Uma boa prática é criar uma pasta `assets` ou `images`.
3.  Substituir a linha acima pelo código Markdown, por exemplo: `![Diagrama de Arquitetura](assets/restaurante-architecture.png)`

---

### Componentes da Arquitetura

#### Usuários (Web / App)
Representa os clientes que interagem com a aplicação do restaurante através de um navegador (web) ou de um aplicativo móvel (app).

#### Load Balancer (ELB)
O **Elastic Load Balancing (ELB)** age como um ponto de entrada para todas as requisições. Sua principal função é distribuir o tráfego de forma uniforme entre as instâncias **EC2** disponíveis. Isso previne que qualquer servidor fique sobrecarregado, garantindo alta disponibilidade e performance.

#### Auto Scaling Group e EC2 Instances
O **Auto Scaling Group** é um serviço que gerencia automaticamente as **EC2 Instances**, que são servidores virtuais onde a aplicação do restaurante é executada. Ele monitora a demanda e adiciona ou remove instâncias (como `app-01` e `app-02`) para garantir que a capacidade de processamento seja sempre suficiente.

#### EBS Volume (Attached)
**Elastic Block Store (EBS)** é um serviço de armazenamento de blocos persistente. Cada instância **EC2** tem um volume **EBS** anexado, funcionando como um disco rígido virtual para armazenar o sistema operacional, logs da aplicação e outros arquivos essenciais.

#### RDS (opcional) - Banco de Dados
O **Relational Database Service (RDS)** é um banco de dados gerenciado. Ele é o local ideal para armazenar dados estruturados e críticos da aplicação, como informações de clientes, pedidos e o menu do restaurante. Sendo um serviço gerenciado, ele simplifica tarefas como backups, atualizações e escalabilidade.

#### S3 (opcional) - Imagens / Arquivos
O **Simple Storage Service (S3)** é um serviço de armazenamento de objetos altamente durável e escalável. Ele é usado para hospedar arquivos não estruturados, como imagens de pratos, fotos de perfil e documentos, que podem ser acessados pela aplicação ou diretamente pelo usuário.

---

### Fluxo da Requisição

1.  O usuário envia uma requisição para o endereço do site ou aplicativo.
2.  O **Load Balancer** recebe a requisição e a encaminha para uma das instâncias **EC2** disponíveis.
3.  A instância **EC2** processa a solicitação, acessando dados do **RDS** (para informações de pedidos, por exemplo) e do **S3** (para carregar imagens).
4.  A resposta é enviada de volta ao **Load Balancer**, que a retorna ao usuário.
5.  Em caso de aumento de tráfego, o **Auto Scaling Group** cria novas instâncias **EC2** para que o **Load Balancer** possa distribuir a carga, mantendo o serviço online e rápido.
