# PESQUISA
*O que é JWT e como funciona?*

O JSON Web Token (JWT) é um padrão aberto (RFC 7519) que permite a transmissão segura de informações entre partes como um objeto JSON. Essas informações podem ser verificadas e confiáveis, pois são assinadas digitalmente. O JWT é amplamente utilizado para autenticação e autorização em aplicações web, permitindo que os usuários façam login e acessem recursos de forma segura.
 A estrutura JWT é uma string codificada que consiste em três partes separadas por pontos com uma estrutura básica formada por header, payload e signature.
 
 
 
 *Como funciona o JWT?*

Um JWT é composto por três partes principais:

    Cabeçalho (Header): O cabeçalho geralmente contém duas partes: o tipo do token (JWT) e o algoritmo de assinatura que está sendo usado, como HMAC SHA256 ou RSA.

    Payload: O payload contém as declarações (claims) que podem incluir informações sobre o usuário e outras informações relevantes. Essas declarações podem ser de três tipos:
        Registered Claims: Conjunto de declarações predefinidas, como iss (emissor), exp (expiração), sub (assunto), entre outras.
        Public Claims: Declarações que podem ser definidas por qualquer um, mas é recomendável que sejam nomeadas de forma a evitar colisões.
        Private Claims: Declarações personalizadas criadas para compartilhar informações entre partes que concordam em usá-las.

    Assinatura (Signature): Para criar a assinatura, você pega o cabeçalho codificado e o payload, e os combina com uma chave secreta (ou uma chave privada, se estiver usando RSA) usando o algoritmo de assinatura especificado no cabeçalho. Isso garante que o token não possa ser alterado sem invalidar a assinatura.
    
  


### 1. Como utilizar o pacote bcryptjs no Node.js?

O `bcryptjs` é um pacote para hash de senhas. Primeiro, instale o pacote:

```bash
npm install bcryptjs
```
0
Para usar, você pode fazer o seguinte:

- **Hashing uma senha**:

```javascript
const bcrypt = require('bcryptjs');

const senha = 'sua_senha';
const saltRounds = 10;

bcrypt.hash(senha, saltRounds, (err, hash) => {
  // Armazene o hash na base de dados
});
```

- **Verificando uma senha**:

```javascript
bcrypt.compare(senha, hash, (err, isMatch) => {
  if (isMatch) {
    // A senha está correta
  } else {
    // A senha está incorreta
  }
});
```

### 2. Como proteger rotas com middleware?

Para proteger rotas, você pode criar um middleware que verifica se o token JWT é válido:

```javascript
const jwt = require('jsonwebtoken');

function authMiddleware(req, res, next) {
  const token = req.headers['authorization'];

  if (!token) return res.status(403).send('Token é necessário.');

  jwt.verify(token, 'sua_chave_secreta', (err, decoded) => {
    if (err) return res.status(401).send('Token inválido.');
    req.user = decoded; // Armazena o usuário decodificado na requisição
    next();
  });
}

// Use o middleware nas rotas
app.use('/rota-protegida', authMiddleware, (req, res) => {
  res.send('Conteúdo protegido!');
});
```

### 3. Como armazenar e utilizar tokens no cliente?

Tokens JWT podem ser armazenados no `localStorage` ou em cookies. Aqui está um exemplo de como armazenar:

```javascript
// Armazenar token no localStorage
localStorage.setItem('token', token);

// Recuperar token do localStorage
const token = localStorage.getItem('token');
```

Se optar por cookies, use a biblioteca `js-cookie` para facilitar:

```javascript
import Cookies from 'js-cookie';

// Armazenar token em um cookie
Cookies.set('token', token);

// Recuperar token do cookie
const token = Cookies.get('token');
```

### 4. Quais são as boas práticas de segurança para usar JWT?

- **Use HTTPS**: Sempre transmita tokens via HTTPS para proteger contra interceptação.
  
- **Chave Secreta Forte**: Utilize uma chave secreta robusta para assinar os tokens.

- **Defina um Tempo de Expiração**: Sempre inclua um tempo de expiração (`exp`) nos tokens para limitar sua validade.

- **Revogação de Tokens**: Considere implementar um sistema para revogar tokens, como uma lista negra.

- **Armazenamento Seguro**: Armazene tokens de forma segura no cliente, evitando exposição a XSS.

- **Verificação de Algoritmos**: Certifique-se de que o algoritmo de assinatura no seu JWT é seguro.

Se precisar de mais detalhes sobre algum ponto específico, é só avisar!
