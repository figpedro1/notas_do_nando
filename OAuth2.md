# OAuth2

## Introdução

### O que é o OAuth2?

OAuth2, que significa Open Authorization (versão 2), é um **PROTOCOLO** de
**AUTORIZAÇÃO** governado pela [RFC 6749][1]. Essas distinções são bastante importantes
e serão exploradas a frente.

O protocolo OAuth2 é especialmente útil quando estamos construindo uma aplicação
utilizando-se da arquitetura de microsserviços, devido a sua centralização da
autenticação e autorização do usuário.

Essa centralização proporciona uma maior conveniência, quando
utilizando SSO (Single sign on), já que o usuário não precisa realizar o login
diversas vezes em diferentes serviços, além de uma maior segurança, já que não é
necessário o tráfego de informações sensíveis do usuário por partes diferentes
da aplicação, ao invés disso se concentrando a um único servidor.

### Distinções importantes

- PROTOCOLO E IMPLEMENTAÇÃO

  Quando se fala em protocolos, significa que os mesmos estabelecem certos
  padrões de comportamento que o implementador do dito protocolo deve ter, mas não descreve
  como deve ser implementado. É análogo a interfaces em linguagens de
  programação com o paradigma de orientação a objeto.

  Já a implementação é justamente a criação da lógica associada a um protocolo,
  interface ou algo que defina comportamentos que a mesma deve ter.

- AUTENTICAÇÃO E AUTORIZAÇÃO

  Os termos autorização e autenticação muitas vezes são usados de maneira
  intercambiável entre si, entretanto, quando falamos de maneira mais técnica, é
  importante fazer a distinção entre esses dois termos.

  - AUTENTICAÇÃO

    A autenticação diz respeito a verificação que é feita para determinar se um usuário é mesmo
    quem ele diz ser. O exemplo mais simples disso é a autenticação por meio de
    nome de usuário e senha, em que o usuário envia as credenciais, e elas são
    comparadas às credenciais guardadas previamente no banco de dados, e, caso
    sejam iguais é possível afirmar que aquele usuário é de fato quem ele diz
    ser.

  - AUTORIZAÇÃO

    A autorização por sua vez, diz respeito a quando o usuário já foi
    autenticado e precisa de acesso a recursos protegidos dos quais ele tem
    permissão para acessar.

## Cargos

O protocolo define 4 cargos:

- _Authorization server_

  O servidor de autorização é onde fica toda a lógica de autenticação e
  autorização, possuindo api's para cada um dos outros cargos realizarem suas
  respectivas funções.

- _Client_

  O cliente é a aplicação que precisa de acesso a recursos protegidos. O seu
  papel é o de obter autorização do servidor de autorização em nome do _resource owner_ (geralmente o
  usuário), caso necessário, ou em seu próprio nome, para então fazer requisições ao servidor de
  recursos com o token de autorização.
  O cliente deve ser registrado no authorization server.

- _Resource server_

  É no servidor de recursos em que vivem as informações ou operações que o
  cliente deseja acessar. O cliente fará uma requisição ao servidor
  de recursos com o token de autorização e então o servidor de recursos fará uma
  solicitação ao servidor de autorização para verificar se aquele token é válido
  e se concede autorização aos recursos que o cliente solicitou. Caso o token
  seja apropriado à solicitação feita o servidor de recursos retornará os
  recursos solicitados.

- _Resource owner_

  O _Resource owner_ geralmente é o usuário final da aplicação e como o próprio
  nome diz, é o dono dos recursos presentes no servidor de recursos. O mesmo
  é quem deve consentir o client usar seus recursos. De agora em diante o
  _Resource owner_ será referido como usuário.

## Fluxos de autorização

O OAuth2 define 5 fluxos de autorização para obtenção dos tokens de acesso,
nesta seção entraremos em detalhes de como cada um deles funciona.

### _Authorization code grant_

Neste fluxo o cliente redireciona o usuário para o endpoint de autorização, onde
o usuário efetuará a autenticação. Então o servidor de autorização redirecionará
o usuário de volta para uma uri de callback previamente registrada da aplicação
cliente, levando consigo um código de autorização.

A aplicação cliente então obtém esse código e realiza uma requisição ao endpoint
de tokens do servidor de autorização com o código de autorização, e recebe de
volta o token de acesso (se o servidor de autorização estiver configurado também
receberá o token de refresh).

Este é o fluxo mais indicado para a maioria dos casos.

![Diagrama do fluxo authorization grant](https://camo.githubusercontent.com/473718269714ff6ac6687ba09c82960e2f414f75c816ceaa7dccdd554a09ac1b/68747470733a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f706e672f64504a485169386d3538526c564f685f5162664173306b384d44716b6c31417755496b347a5a5a33663172394c3158762d334d44445769584b4666544c335a61465f634947726d6870514d6b5855507931616c453164394e434958396a4c545034536f70306b706c334b64756f6531397a373453507a35416f73414a5f4f6950357946307a59416571545a4c744c55533134613455714a324a72595f4f6235615a45343539544c324b674c35732d3147786930774c574f585f61303748503835784c384f3535547a52674e75764e727074774675696a76465a757472693674316b5f4e7342314e4e6645307a46615a5549304f555a6634553849506e37726c683449666c737667696e70424838745433695079535154397079494f364f706359765f513854744334456463436e336e2d656a71646e507444524d38797a76536e394d7679564d4b642d395155653643556e655364503330474675593855375561364844564d6c64745a3562474c314b6658535f73636e676d63376467775852744c2d6e5f6833515f6541494a39374c7352524250636667775f706c78306d3030)

### _Implicit grant_

Este fluxo é bastante semelhante ao _Authorization code_, já que o cliente redireciona o usuário para o endpoint de autorização, onde
o usuário efetuará a autenticação, entretanto, neste fluxo o servidor retornará
diretamente o token de acesso ao invés do código para obtenção do mesmo.

Este fluxo costuma ser utilizado em aplicações SPA (Single page application), em
que o client vive no browser do usuário.

![Diagrama do fluxo implicit grant](https://camo.githubusercontent.com/2778ae3e529245332186b827f8b4d7d35bbc1d559cbac461547597d000a54c96/68747470733a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f706e672f664c4844516d386e34427478412d504b44695a2d306f636b6a685155326268664a4b5874666e685139594b50354b662d563353486e5f5748734538787036345374787074395a66713932717259674a69474b325f50464a74374779705436505a516d485f774333424a67433545374f4f4669534a69556e544a304f4b3876357970502d697676396b364e796672304a4a666774674777347a4e7466313452356c35764b6549785933364a52414f61744151355a43666b374a556d79664b654342587050626f7939336a5136726955664e316b67346562675839624f6e325965596773706457763551345a6657354e45714d78486d364e4374354b5461413050316e4c59717a4c654b76513033314a3173596a5047724a4b7731775f6334744e2d3864615a496946346f6a4b74516c674e757777784f6b75464d53536e7a70547942762d4438736d7745334f524f694b796d455f70586a466e5a2d7753656659386d2d61554852613770366b6856575a4f556c76715f684f75477a696437555259354c314b65597246756b476e4e39733450495a5230735f6b52596f554f5f4d505950485a594267395f736a5f)

### _Resource Owner Password Credentials Grant (Password Grant)_

Neste fluxo o cliente efetua uma requisição ao endpoint de tokens com o nome de
usuário e senha do usuário, e o servidor retorna diretamente o token de acesso.

Este fluxo é altamente desencorajado e deve ser reservado apenas para clientes
de altíssima confiança, como por exemplo uma aplicação mobile feita pelos donos
do sistema.

![Diagrama do fluxo password grant](https://camo.githubusercontent.com/9b8b018dca87b54f014bf5dd1be9675134701fabcf3eb8a4081a59bc9aeb23a7/68747470733a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f706e672f665039485169436d33385256556d657a3957497032764a494230566577374f3161735a4b424458335358694f45535445697a466939416b646b47384a6f413356616f467a2d514b5f7950775a664a59755145713634657376714b58625859446f4535454643455341373251463141324c365f45594a2d527879716b5676424e69426678516854655164597070353659305a64313037767154554c4645345978627430524d4b3736504533466c5a416155777a47687a57475a7834496962357a344a756a665f594a6a455075326b4779524e563367714f746b67616b644c4b4a6f5a3748314d6a765a5579744949505173787634504e693852555a325078562d6f4c4443333744784a65645134387875497961636951584d4e3764562d627074777556554d5f573030)

### _Client credentials grant_

Neste fluxo de autenticação o cliente envia suas credenciais (client_id e
client_secret) ao endpoint de tokens e recebe o token de acesso.

Este fluxo é usado quando o client não necessita da autorização do usuário para
utilizar um recurso.

### Refresh token grant

Neste fluxo de autenticação, o cliente envia para o endpoint de tokens o token
de atualização, recebendo em troca um novo token de acesso e um novo token de
atualização.

Este fluxo é utilizado quando já se obteve autorização prévia com um
_access_token_ e _refresh_token_ mas o token de acesso já expirou.

O mesmo, é utilizado pois o token de acesso não costuma ser armazenado, sendo
verificado apenas através de sua assinatura, por isso deve ter um tempo de vida
bastante curto (Entre 5 minutos e 1 hora), já que não pode ser revogado.
Então para que o usuário não precise
continuamente fazer login na aplicação novamente é criado um token de
atualização, que é armazenado no banco de dados e por isso, pode ser revogado a qualquer
momento.

![Diagrama para os fluxos refresh token e client credentials](https://camo.githubusercontent.com/ebdfcecdf04f683039c82545fe46561f0f6a02a6ca29dcb02b1f7368449e86df/68747470733a2f2f7777772e706c616e74756d6c2e636f6d2f706c616e74756d6c2f706e672f70504a31496944303438526c46694d534969386f526f33494d636c726641576c6932475a4e476f78435a6c6a47565258684a414b3334474a4154482d6a6f4a61757a2d507350626479496e6a3445796c30316d673367525a714542736557394147516d4d6a4e77316e4c4f735953453762763059684462375753445f4c6d4b696e4f585745682d4242683677726149516763616550745839526e47312d5f6d6671476f5577394b66527a4a4b76714837637a5763446f6632683952724b3735696379664859516e656c3669744453385a397735366744714e4577536a446d493876563257385a446441626673575141433964656374583774346b63526d48676a706776366a704e32565470764268495150794d53364432414a5a4c3743397448574c6f5249736c6c366b395f68393678387a5238754167546d53467872795a777066786c76724b78667a582d522d7a46)

## Endpoints do servidor de autorização

_Nota: O protocolo não define uris específicas para as implementações, fica a
critério do desenvolvedor decidir a uri mais adequada_

### Endpoint autorização

É utilizado nos fluxos _implicit_ e _Authorization code_, e é para onde o usuário é redirecionado pelo client, quando o
mesmo deseja obter autorização em seu nome. Nele o usuário fará a autenticação
e **pode** ser exibida uma tela de consentimento sobre as autorizações que o
client está solicitando.

#### Informações técnicas

**Método:** GET

**Headers:**

| Header       | valor                               |
| ------------ | ----------------------------------- |
| Content-Type | "application/x-www-form-urlencoded" |

**Parâmetros:**

| Parâmetro        | valor                                                                                                                                                                        |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| response_type    | "code", caso esteja usando o fluxo _Authorization code_ ou "token", caso esteja utilizando o fluxo _implicit_                                                                |
| client_id        | id do cliente que deseja acesso aos recursos protegidos                                                                                                                      |
| redirect_uri     | A uri para onde o usuário deve ser redirecionado após realizar a autenticação. _Nota: O client deve registrar todas suas uris de redirect, para evitar ataques de terceiros_ |
| scope            | Uma lista delimitada por espaços para os escopos (autorizações) para os quais o cliente deseja autorização                                                                   |
| state (Opcional) | Um valor que será retornado ao cliente para ajudar o client a levar o usuário para o estado adequado da aplicação                                                            |

### Endpoint token

Esse endpoint é utilizado por todos os fluxos com exceção do _implicit_ para
obter os tokens de acesso e de refresh, variando os parâmetros necessários de
acordo com cada tipo de fluxo de autorização. Os parâmetros devem ser enviados
no corpo da requisição de maneira x-www-form-urlencoded.

#### Informações técnicas

**Método:** POST

**Headers:**

| Header        | valor                                                                                                         |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| Content-Type  | "application/x-www-form-urlencoded"                                                                           |
| Authorization | "Basic" + <base64*url_safe(client_id:client_secret)> *Nota: Somente obrigatório no fluxo client_credentials\* |

**Parâmetros para o fluxo _Authorization code_:**

| Parâmetro  | valor                                                      |
| ---------- | ---------------------------------------------------------- |
| grant_type | "authorization_code"                                       |
| code       | Código de autorização obtido através do endpoint authorize |
| client_id  | Id do cliente                                              |

**Parâmetros para o fluxo _Client credentials_:**

| Parâmetro  | valor                                                           |
| ---------- | --------------------------------------------------------------- |
| grant_type | "client_credentials"                                            |
| scope      | Escopos que o cliente deseja acessar _Nota: Parâmetro opcional_ |

**Parâmetros para o fluxo _password_:**

| Parâmetro  | valor                                                          |
| ---------- | -------------------------------------------------------------- |
| grant_type | "password"                                                     |
| username   | Nome de usuário                                                |
| password   | Senha do usuário                                               |
| scope      | Escopos que o cliente deseja acessa _Nota: Parâmetro opcional_ |

**Parâmetros para o fluxo _refresh token_:**

| Parâmetro     | valor                                   |
| ------------- | --------------------------------------- |
| grant_type    | "refresh_token"                         |
| refresh token | Token de atualização previamente obtido |

### Endpoint introspecção de token

Esse endpoint é usado para realizar a validação do token, verificação de
escopos, dentre outras coisas

#### Informações técnicas

**Método:** POST

**Headers:**

| Header        | valor                                                 |
| ------------- | ----------------------------------------------------- |
| Content-Type  | "application/x-www-form-urlencoded"                   |
| Authorization | "Basic" + <base64\*url_safe(client_id:client_secret)> |

**Parâmetros:**

| Parâmetro | valor                                                 |
| --------- | ----------------------------------------------------- |
| token     | Token de acesso que se deseja realizar a introspecção |

[1]: <https://datatracker.ietf.org/doc/html/rfc6749> "RFC 6749"
