
## What is Mutation testing?

![image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/74xrpgja3vl50ijpvxfx.png) 

Pitest makes a mutation in your project, it allows you to ensure a real coverage margin within your tests by making a “copy” of the project and inserting errors to see if your tests will fail after the mutation. When the roll fails the mutant is killed. If any mutants survive it means you need to do more unit tests, the live mutants serve as input to create more tests.
## Okay, but how does this help me with the quality of my code?

![image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lswmr2pp6tnea5hs8k1n.png) 

Due to the classic rush that developers need to go in to deliver a certain project, it turns out that testing is done more to increase code coverage, but this is not correct, the tests come with the objective of providing more quality of the code. that you are delivering to a client or consumer and also document it so that another developer can better understand your code, later on you will understand where I want to go :)

## What technologies will we use?

![image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gwddh9mzr7fub83gdvag.png) 

We will need to have the following technologies in our project:

1. [Java 8+](https://www.java.com/pt-BR/download/help/whatis_java.html): Java Programming Language in version 8+
2. [Junit 5](https://junit.org/junit5/): Test development framework
3. [Maven](https://maven.apache.org/): Build automation tool
4. [Pitest](https://pitest.org/): Tool for performing mutant tests
5. [Spring](https://spring.io/):programming framework


## Setting up the project:

![image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vc5dbouy55a2xq6iyn5z.png) 

In order for us to use **pitest**, we need to use Junit 4 or higher.

```xml
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-launcher</artifactId>
    <version>1.7.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.7.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <version>5.7.1</version>
    <scope>test</scope>
</dependency>
```
It will also be necessary to add the **pitest** plugin to the project.

```xml
            <plugin>
                <groupId>org.pitest</groupId>
                <artifactId>pitest-maven</artifactId>
                <version>1.4.11</version>
                <dependencies>
                    <dependency>
                        <groupId>org.pitest</groupId>
                        <artifactId>pitest-junit5-plugin</artifactId>
                        <version>0.8</version>
                    </dependency>
                </dependencies>
                <configuration>
                    <targetClasses>
                        <param>YourClassPackage.*</param>
                    </targetClasses>
                    <targetTests>
                        <param>YourTestPackage.*</param>
                    </targetTests>
                </configuration>
            </plugin>
```
**Note**: There are other settings you can insert in the plugin according to your preference, such as excluding a certain class or package that will not be tested:

```xml
<excludedClasses>
 <param>PackageYourClasses.*</param>
 <param>PackageYourClasses.ImClasses</param>
</excludedClasses>
```

## Revealing the failure to test for coverage only:

![image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9txnkmi5av2hmkoojqoq.png)

- Within our project we have a class called **UsuarioTranslator**

- In our method we will receive as parameters a String(name) and an Integer(age)

- After that we will make the instance of the **User** class and we will set the parameters received in their respective attributes

- Soon after, we will return the user
- 
```java
public final class UsuarioTranslator {

    private UsuarioTranslator() {}
    
    public static Usuario of(final String nome, final Integer idade) {
        final Usuario usuario = new Usuario();
        
        usuario.setNome(nome);
        usuario.setIdade(idade);
        
        return usuario;
    }
}
```
Ok, after we understand the purpose of our method, let's go to our test class, **UsuarioTranslatorTest**:

```java
class UsuarioTranslatorTest {

    @Test
    @DisplayName("Deverá retornar Usuario - Quando sucesso")
    void of_Usuario_QuandoSucesso() {
        final Integer idade = 10;
        final String nome = "Dudu";

        final Usuario usuario = UsuarioTranslator.of(nome, idade);

        Assertions.assertNotNull(usuario);
        Assertions.assertEquals(nome, usuario.getNome());
    }
}
```
As we can see, we called our class's method and made the following checks:
- We verified that the return object is not null
- We verified that the expected *name** is the same as the name entered in the **user**

Let's check our coverage?



Mas afinal, nesse exemplo simples não tivemos nenhum problema, onde estava o nosso erro?

Exatamente, esquecemos de fazer a validação se o atributo de **idade** foi settado corretamente, vamos ver o que o **Pitest** tem a dizer?

## Entering more into the tool:

![image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8teleik9kxgyusxvhci8.png)

So that we can run **Pitest** just run the following command in your project's terminal:

```xml
mvn org.pitest:pitest-maven:mutationCoverage

```
Or run the plugin directly in your IDE by clicking on the **pitest:mutationCoverage** option, the choice is yours :)

![Captura de tela de 2021-03-16 23-41-57](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h1ke2dm7umue1kxfbvbq.png)

Após fazer a execução e tiver configurado direitinho o seu projeto até aqui, você irá ter o seguinte resultado:

![Captura de tela de 2021-03-16 23-51-46](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xfv8gcr776fmuroanng5.png) 

As we can see above, **Pitest** generated 5 mutations and with our test we managed to kill only **3**, but don't worry, for you to have a more analytical view, **Pitest* * generates a report for you, so we can access it, just follow the following path:
- target
- pit-reports
- index.html

![Captura de tela de 2021-03-16 23-54-32](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6vqjsjpsrlyi9g4aljuj.png)

As we can see, **Pitest** took our line from user.setIdade(age); and said if we remove the call from that line, the result will be the same and we know that's not true, right? :)

So now we just go back to our test class and make the necessary changes.


 

