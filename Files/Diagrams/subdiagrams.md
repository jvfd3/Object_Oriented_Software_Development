# Diagramas

## UML

### Comportamento

#### **Estados** <!-- (3) -->
<!-- Chat GPT -->
> O diagrama de estados UML (Unified Modeling Language) é um tipo de diagrama que é usado para representar os diferentes estados que um objeto pode assumir durante o seu ciclo de vida e as transições entre esses estados. Ele é usado para modelar o comportamento de um sistema ao longo do tempo e pode ser útil para entender como um objeto reage a eventos externos e internos.
>
> Um diagrama de estados UML consiste em um conjunto de estados e transições. Cada estado representa uma condição ou um comportamento de um objeto, enquanto cada transição representa uma mudança de um estado para outro.
>
> Os estados podem ser divididos em três categorias:
>
> 1. Estados ativos: são os estados em que o objeto está realizando alguma ação. Por exemplo, um objeto pode estar em um estado ativo enquanto estiver processando um pedido ou enviando uma mensagem.
> 2. Estados passivos: são os estados em que o objeto está aguardando algum evento externo ou interno para mudar de estado. Por exemplo, um objeto pode estar em um estado passivo enquanto aguarda uma resposta de outro objeto.
> 3. Estados finais: são os estados em que o objeto não pode mais mudar de estado e o processo é encerrado.
>
> As transições são representadas por setas que ligam os estados. Cada transição tem um nome que descreve o evento que causou a mudança de estado, bem como uma condição que deve ser atendida para que a transição ocorra.

##### Sistema Geral

[Link ao diagrama][ImgUMLComportamentoEstados1]

###### **Imagem do diagrama: UML - Estados - Sistema Geral**

```mermaid
stateDiagram-v2

[*] --> SistemaEmExecução
SistemaEmExecução --> [*]
state SistemaEmExecução {
    state if_state <<choice>>
    state if_state2 <<choice>>
    state fork_state_main <<fork>>
    state join_state1 <<join>>
    Open: Abrindo o Site
    Close: Fechando o Site
    NotLogged: Deslogado
    Logged: Logado
    state AcessandoSite {
        state join_state_site <<join>>
        state fork_state_site <<fork>>
        Alterando_Configurações: Alterando Configurações
        Alterando_Itens: Alterando Itens
        Alterando_Conta: Alterando Conta
        [*] --> fork_state_site
        fork_state_site --> Alterando_Configurações: Alterar Configurações
        fork_state_site --> Alterando_Itens: Alterar Itens
        fork_state_site --> Alterando_Conta: Alterar Conta
        Alterando_Conta --> join_state_site
        Alterando_Configurações --> join_state_site
        Alterando_Itens --> join_state_site
        join_state_site --> [*]
    }
    state Cadastrando {
        state join_state3 <<join>>
        state fork_state4 <<fork>>

        AuthName1: Verificando nome
        AuthPswd2: Autenticando Senha
        [*] --> fork_state4: Dados iniciais
        fork_state4 --> AuthName1: Dados do nome
        fork_state4 --> AuthPswd2: Dados da senha
        AuthName1 --> join_state3: Resultado da verificação
        AuthPswd2 --> join_state3: Resultado da autenticação
        join_state3 --> [*]: Resultado das autenticações
    }
    state Autenticando {
        state join_state2 <<join>>
        state fork_state3 <<fork>>

        AuthName: Verificando nome
        AuthPswd: Autenticando Senha
        [*] --> fork_state3: Dados iniciais
        fork_state3 --> AuthName: Dados do nome
        fork_state3 --> AuthPswd: Dados da senha
        AuthName --> join_state2: Resultado da verificação
        AuthPswd --> join_state2: Resultado da autenticação
        join_state2 --> [*]: Resultado das autenticações
    }
    signedUp: Usuário Cadastrado
    NotSignedUp: Usuário não Cadastrado
    [*] --> Open
    Cadastrando --> if_state2
    if_state2 --> NotSignedUp: Não autenticado
    if_state2 --> signedUp: Autenticado
    signedUp --> join_state1
    NotSignedUp --> join_state1
    join_state1 --> NotLogged
    Open --> NotLogged
    NotLogged --> fork_state_main
    fork_state_main --> Cadastrando: Cadastrar
    fork_state_main --> Autenticando: Logar
    Autenticando --> if_state
    if_state --> NotLogged : Não autenticado
    if_state --> Logged: Autenticado
    Logged --> AcessandoSite
    AcessandoSite --> Close
    Close --> [*]
}
```

##### Alterando Configurações

[Link ao diagrama][ImgUMLComportamentoEstados2]

###### **Imagem do diagrama: UML - Estados - Alterando Configurações**

```mermaid

stateDiagram-v2

[*] --> AlterandoConfigurações
AlterandoConfigurações --> [*]
state AlterandoConfigurações {
    direction LR
    state fork_state_main <<fork>>
    state join_state_main <<join>>
    state Alterar_Linguagem {
        direction LR
        EN: Sistema em inglês
        PTBR: Sistema em português
        state fork <<fork>>
        state join <<join>>
        [*] --> fork
        fork --> EN
        fork --> PTBR
        EN --> PTBR: Alterando linguagem
        PTBR --> EN: Alterando linguagem
        EN --> join
        PTBR --> join
        join --> [*]: Confirmar escolha
    }
    state Alterar_Tema {
        direction LR
        Dark: Sistema no modo escuro
        Light: Sistema no modo claro
        state fork_Tema <<fork>>
        state join_Tema <<join>>
        [*] --> fork_Tema
        fork_Tema --> Dark
        Dark --> Light: Alterando tema
        fork_Tema --> Light
        Light --> Dark: Alterando tema
        Dark --> join_Tema
        Light --> join_Tema
        join_Tema --> [*]: Confirmar escolha
    }

    [*] --> fork_state_main
    fork_state_main --> Alterar_Linguagem
    fork_state_main --> Alterar_Tema
    fork_state_main --> join_state_main: Não alterar nada
    Alterar_Linguagem --> join_state_main: Linguagem definida
    Alterar_Tema --> join_state_main: Tema definido
    join_state_main --> [*]
}
```

##### Alterando Itens

[Link ao diagrama][ImgUMLComportamentoEstados3]

###### **Imagem do diagrama: UML - Estados - Alterando Itens**

```mermaid

stateDiagram-v2

[*] --> AlterandoItens
AlterandoItens --> [*]
state AlterandoItens {
    direction LR
    state fork_state_main <<fork>>
    state join_state_main <<join>>

    [*] --> fork_state_main: Solicitação de alteração
    C: Criando Item
    R: Lendo Item
    U: Atualizando Item
    D: Deletando Item
    fork_state_main --> C: ItemCreate
    fork_state_main --> R: ItemRead
    fork_state_main --> U: ItemUpdate
    fork_state_main --> D: ItemDelete

    C --> join_state_main
    R --> join_state_main
    U --> join_state_main
    D --> join_state_main

    join_state_main --> [*]: Alteração concluída
}
```

##### Alterando Conta

[Link ao diagrama][ImgUMLComportamentoEstados3]

###### **Imagem do diagrama: UML - Estados - Alterando Conta**

```mermaid
stateDiagram-v2
[*] --> Alterando_Conta_2
Alterando_Conta_2 --> [*]
state Alterando_Conta_2 {
    direction LR
    state fork_state_main <<fork>>
    state join_state_main <<join>>
    state Alterando_Imagem {
        UserAddImg: Adicionar Imagem
        UserModImg: Modificar Imagem
    }
    state Alterando_Email {
        UserAddEmail: Adicionar Email
        UserModEmail: Modificar Email
    }
    state Alterando_Senha {
        UserModPswd: Modificar Senha
    }
    state Alterando_Nome {
        UserModName: Modificar Nome
    }
    state Alterando_Conta {
        UserDelete: Deletar Conta
        UserLogout: Deslogar da Conta
    }
    
    [*] --> fork_state_main
    fork_state_main --> Alterando_Imagem
    fork_state_main --> Alterando_Email
    fork_state_main --> Alterando_Senha
    fork_state_main --> Alterando_Nome
    fork_state_main --> Alterando_Conta

    Alterando_Imagem --> join_state_main
    Alterando_Email --> join_state_main
    Alterando_Senha --> join_state_main
    Alterando_Nome --> join_state_main
    Alterando_Conta --> join_state_main
    join_state_main --> [*]
}
```

## Links

[ImgUMLComportamentoEstados1]: https://github.com/
[ImgUMLComportamentoEstados2]: https://github.com/
[ImgUMLComportamentoEstados3]: https://github.com/

