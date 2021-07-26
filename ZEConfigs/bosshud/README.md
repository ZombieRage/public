# ZombieRage - ZEConfigs - BossHud
Observação: **Abra as imagens em outra aba para uma melhor visualização!**

## Etapa 1
Antes de tudo precisamos conhecer como é a aparência de uma config de boss hud, para isso existe uma cfg modelo da qual você pode ate usar como base na criação da config, [modelobase.cfg](/ZEConfigs/bosshud/modelobase.cfg).

## Etapa 2
Tendo conhecido o formato de uma cfg, vale ressaltar que a cfg de modelo base possui 4 tipos de boss, nos quais são:
- Boss1 <- Boss sem barra de hp custom. 
- Boss2 <- Boss com barra de hp e que possui output de OnHitMin no math_counter da barra.
- Boss3 <- Boss com barra de hp e que possui output de OnHitMax no math_counter da barra.
- Boss4 <- Boss que usa a vida da propia entidade de hitbox como breakable ou physbox.

<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_1.png" /><br>
  Exemplo de barra de hp custom em um mapa.
</p>

Vamos agora a uma leve explicação da função daqueles parâmetros, incluindo o template total da cfg:
```
"math_counter" // nunca será alterado, então não precisa se preocupar com o mesmo.
{
  "config" // parâmetros usados globalmente para todos os boss na cfg em questão. Tal key é opcional e pode ser ignorada/apagada.
  {
    "HitMarkerOnly"             "0"	// parâmetro global para desativar o hud, ficando apenas os hitmarker e ranking (util para mapas que já possuem hud integrado). 1 para ativar (vem desativado de padrão).
    "BossBeatenShowTopDamage"   "1"	// parâmetro global para mostrar o top 3 de dano ao final de todas boss fights. 0 para desativar (vem ativado de padrão). 	
    "BossBeatenRewardTopDamage" "1"	// parâmetro global para recompensar o top 3 de dano ao final de todas boss fights. 0 para desativar (vem ativado de padrão).
    "BossDamageMoney"           "1"	// parâmetro global para especificar se o o dano causado os bosses vai ser convertido para dinheiro. 0 para desativar (vem ativado de padrão).
  }
  "0" // Exemplo de config para bosses do tipo 'math_counter'
  {
    "HP_counter"      "" // targetname do math_counter principal do boss, geralmente é o math_counter que gerencia os outputs dos outros math_counter's secundários.
    "HPbar_counter"   "" // targetname do math_counter responsável pelo progresso da barra de hp custom do boss. 	
    "HPbar_min"       "" // valor do parâmetro "min" mostrado na entidade do math_counter responsável pelo progresso da barra de hp custom do boss. 	
    "HPbar_max"       "" // valor do parâmetro "max" mostrado na entidade do math_counter responsável pelo progresso da barra de hp custom do boss. 	
    "HPbar_default"   "" // valor do parâmetro "startvalue" mostrado na entidade do math_counter responsável pelo progresso da barra de hp custom do boss. 		
    "HPbar_mode"      "" // modo de output apresentado no math_counter responsável pela barra de hp, 1 para "OnHitMin" e 2 para "OnHitMax".	
    "CustomText"      "" // nome do boss a ser mostrado no hud de bosshud do server.
  }
  "1" // Exemplo de config para bosses do tipo 'breakable'
  {
    "Type"            "breakable" // especificação de boss que não usa math_counter para progredir o própio HP.
    "BreakableName"   ""          // targetname da entidade que administra a progressão de HP do boss, geralmente nesse tipo de boss, os tipos de entidades responsáveis por isso tendem a ser "func_breakable", "func_physbox" ou "func_physbox_multiplayer".
    "CustomText"      ""          // nome do boss a ser mostrado no hud de bosshud do server.
  }
}
```

## Etapa 3
Levando em consideração que você ja possui os programas necessários para realizar a criação da config e o conhecimento básico, inicie o programa **VIDE**:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/geral_1.png" width="75%" /><br>
</p>

Após isso vá ate a aba **TOOLS** e logo em seguida **ENTITY LUMP EDITOR**:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/geral_2.png" width="75%" /><br>
</p>

Após isso vá ate a aba **FILE**, depois **OPEN**, localize o mapa em questão e selecione-o:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/geral_3.png" width="75%" /><br>
</p>

### Criando uma cfg para boss do tipo math_counter
Após isso todas as entidades do mapa selecionado vão ser listadas, porém o que buscamos são apenas as entidades de vida dos bosses ou seja `"math_counter"` ou `"func_breakable/physbox"` por exemplo, então vamos procurar por um destes através do **FILTER OPTIONS**:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_5.png" width="75%" /><br>
</p>

Realizamos a busca e achamos alguns bosses que progridem sua vida através de `math_counter`, se temos a informação de que o boss não possui barra de HP custom, então todos os parâmetros **"HPbar"** na cfg serão desconsiderados, nesse caso você so vai usar o targetname do counter de HP principal e o nome do boss para este tipo de boss.<br>
Agora se o Boss possui barra de HP custom que é o caso do mapa mostrado acima, então vamos precisar achar o `math_counter` principal e o `math_counter` que progride a barra de hp, se acharmos estes 2, teremos tudo de que precisamos.

Vamos procurar então pelas informações que precisamos para este tipo de boss, já que eu procuro por um boss específico chamado *"pirate"*, vou digitar na barra de filtragem por *"pirate_hp"* para tentar achar o `math_counter` principal que é o counter que administra os outros:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_6.png" width="75%" /><br>
</p>

Como se pode observar pelos outputs deste `math_counter`, ele é o nosso counter principal pois ele administra outro counter de hp backup e outro counter responsável por certos eventos do boss, logo vamos adicionar o targetname *"pirate_counter"* no parâmetro **"HP_counter"** da nossa cfg.

Agora precisamos do `math_counter` responsável pela barra de hp, achar esse counter é a parte mais facil pois este counter é responsável por matar todas as entidades do boss quando a barra de hp do mesmo é zerada, logo vamos procurar por um counter que possua outputs responsáveis por matar as entidades do boss:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_7.png" width="75%" /><br>
</p>

Como se pode observar pelos outputs de Kill deste `math_counter`, ele é o counter responsável pela barra de hp, logo vamos adicionar o targetname *"pirate_hp_iterations"* ao parâmetro **"HPbar_counter"** da nossa cfg.

Agora so falta o resto das informações sobre os parâmetros **"HPbar"**, estes que serão encontrados também no `math_counter` responsável pela barra de HP:
```
  "HPbar_min"  será definido pelo valor "0" através do parâmetro "min" 
  "HPbar_max"  será definido pelo valor "5" através do parâmetro "max" 
  "HPbar_default"  será definido pelo valor "5" através do parâmetro "startvalue"
  "HPbar_mode" será definido pelo valor "1" por ter outputs do tipo "OnHitMin"
  "CustomText"  será o definido pelo nome do nosso boss que será "pirate"
```

Pronto, seguindo estes passos ate o momento, você finalizou a cfg de um boss que usa barra de hp custom.

### Criando uma cfg para boss do tipo breakable
Caso o boss não utilize `math_counter`, ele será do tipo **"breakable"**, ou seja precisamos achar um boss que administre a própia vida através de um `func_breakable`, `func_physbox` ou `func_physxbox_multiplayer`:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_8.png" width="75%" /><br>
</p>

Realizamos a busca e achamos um boss que progride sua vida através de `func_breakable`, então ja podemos especificá-lo como tipo **"breakable"** juntamente com o seu targetname na nossa cfg.

### Finalizando a cfg dos bosses
Enfim todas informações de que precisamos para esse tipo de boss apareceram, pois sabemos o targetname e sabemos que tipo de boss é, então vamos anotar na nossa config:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_9.png" width="75%" /><br>
</p>

Agora so falta o nome do boss para inserir no parâmetro **"CustomText"**, dá para descobrir isso facilmente através de uma leve pesquisada com o targetname do breakable no **FILTER OPTIONS**:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_10.png" width="75%" /><br>
</p>

Como se pode observar, achamos uma entidade chamada BossEnds e ele possui um output com uma mensagem de chat nos dizendo o nome do boss, *BUTTERFLY*.

## Etapa 4
Agora possuimos todas as informações de que precisamos acerca dos determinados tipos de boss.
Vamos então fazer uma copia local da nossa [modelobase.cfg](/ZEConfigs/bosshud/modelobase.cfg) e colocar todas as informações de que temos sobre os Bosses.

Você deve fazer isso para todos os bosses do mapa, quando finalmente terminar o trabalho, você deve salvar em formato CFG com o nome do mapa em questão, por exemplo **ze_shroomforest_p6.cfg**:
<p align="center">
  <img src="https://cdn.zrage.cf/file/zrgbrasil/misc/githubfiles/public/bosshud_11.png" width="75%" /><br>
</p>
