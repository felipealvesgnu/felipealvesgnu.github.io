---
author: felipealvesgnu
title: Serialização e Desserialização de Objetos em Java
#classes: wide
categories:
- Java
---

**Serialização**

Suponhamos que você tivesse um programa, um jogo com uma aventura fictícia que precisasse de mais de uma sessão para ser concluído. Conforme o jogo progride, os personagens ficam mais fortes, fracos, inteligentes, etc. E coletam e usam (e perdem armas). Você não quer iniciar do zero sempre que iniciar o jogo ? Portanto precisa de uma maneira de salvar o estado dos personagens e uma maneira de restaurá-lo quando voltar ao jogo.

1. Opção
  * Grave os objetos serializados dos personagens em um arquivo.

2. Opção 	
  * Grave um arquivo de texto simples.


**Gravando um objeto serializado em um arquivo**

Aqui estão as etapas para serializarmos (salvarmos) um objeto.

```java
//1 - Crie um objeto FileOutputStream
 FileOutputStream fileStream = new FileOutputStream("MyGame.ser");
 //2 - Crie um ObjectOutputStream
 ObjectOutputStream os = new ObjectOutputStream(fileStream);
 //3 - Grave os objetos
 os.writeObject(personagemUm); //Serializa os objetos referenciados por
 os.writeObject(personagemDois);//personagemUm,personagemDois,personagemTres
 os.writeObject(personagemTres); //e grava no arquivo myGame.ser
 //4 - Feche ObjectOutputStream
 os.close;
```

**Transferência de dados do streams de um local para outro**

A API de E/S Java tem streams de **conexão** que representam conexões com destinos e origens como arquivos ou
soquetes de rede, stream de **cadeia** que só funcionam quando encadeados a outros streams.

Geralmente, são necessários dois streams para que algo útil possa ser feito - um para representar a conexão
e outro para chamar métodos. Pois os streams de conexão costuma estar em um nível muito baixo.
FileOutputStream (um stream de conexão), por exemplo, tem métodos para a gravação de bytes. Mas não queremos
gravar bytes! Queremos gravar objetos, portanto, precisamos de um stream de _cadeia_ de nível mais alto. Veja descrição na figura 1.

<figure>
    <a href="/assets/images/serializacao/serialization.jpg"><img src="/assets/images/serializacao/serialization.jpg"></a>
    <figcaption>Figura 1</figcaption>
</figure>


**Implemente _Serializable_**

Serializable é considerada como uma interface _marcadora_ ou de _tag_, porque não tem nenhum um método a implementar. Sua única finalidade é anunciar que a classe que está implementando pode ser serializada. Ou seja os objetos desse tipo poderão ser salvos através do mecanismo de serialização. (Se sua superclasse "FOR-UM" tipo serializable, você também  será). Segue exemplo abaixo.

```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class Box implements Serializable {

 private int largura;
 private int altura;

 public static void main(String[] args) {
  Box myBox = new Box();
  myBox.setLargura(50);
  myBox.setAltura(20);

   try { //operação de E/S pode lançar excessões.
     FileOutputStream fs = new FileOutputStream("foo.ser");//caso não encontre cria novo arquivo chamado foo.ser
     ObjectOutputStream os = new ObjectOutputStream(fs); //fs encadeado ao stream de conexão
     os.writeObject(myBox);
     os.close();
   }catch (Exception e) {
     e.printStackTrace();
   }
 }

 public void setLargura(int largura) {
   this.largura = largura;
 }

 public void setAltura(int altura) {
   this.altura = altura;
 }
}
```

**Desserialização: restaurando um objeto**

O objetivo da serialização de um objeto é podermos restaurá-lo a seu estado original em algum momento posterior, em uma execução diferente da JVM (que pode até ser a mesma que estava sendo executada no momento em que o objeto foi serializado). A desserialização é muito parecida com a serialização ao contrário.

Aqui estão as etapas para desserializarmos (abrir) um objeto.

```java
//1 - Crie um objeto FileInputStream
 FileInputStream fileStream = new FileInputStream("MyGame.ser");
 //2 - Crie um ObjectInputStream
 ObjectInputStream os = new ObjectInputStream(fileStream);
 //3 - Leia os objetos
 Object um = os.readObject();
 Object dois = os.readObject();
 Object tres = os.readObject();
 //4 - Converta os objetos
 GamePersonagem duende   = (GamePersonagem) um;
 GamePersonagem duende_2 = (GamePersonagem) dois;
 GamePersonagem magico   = (GamePersonagem) tres;

 //5 - Feche ObjectInputStream
 os.close();
````

**O que acontece durante a desserialização?**

Quando um objeto é desserializado, a JVM tenta reconstituí-lo criando um novo objeto no acervo que tenha o mesmo
estado que objeto serializado tinha na hora em que foi serializado. Exceto pelas variáveis transientes,
que são reconstituídas com nulo(para referências de objeto) ou com valores primitivos padrão.

Lembrando sempre que se desserialização for em um local diferente (outro sistema por exemplo)  deverá existir a cópia da classe de origem (que foi utilizada para fazer a serialização anteriormente)  para que seja feita a desserialização desse objeto que está sendo manipulado, conforme ilustra abaixo a figura 2.

<figure>
    <a href="/assets/images/serializacao/desserialization.jpg"><img src="/assets/images/serializacao/desserialization.jpg"></a>
    <figcaption>Figura 2</figcaption>
</figure>

PONTOS CHAVES

- Uma variável de instância com a palavra-chave ```transient``` se quiser que a serialização a ignore.

- Durante a desserialização, a classe de todos os objetos da ramificação deve estar disponível na JVM.

Referências Bibliográficas

 *SIERRA, Kathy; BATES, Bert - Use a Cabeça Java - Editora Alta Books, Rio de Janeiro, 2010.*
