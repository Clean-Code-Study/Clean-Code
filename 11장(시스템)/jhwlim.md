# ๐ 11์ฅ. ์์คํ

## ๐ ์์ฝ

- ์์คํ ์์ค์์๋ ๊นจ๋ํจ์ ์ ์งํ๋ ๋ฐฉ๋ฒ์ ๋ค๋ฃจ๊ณ  ์๋ค.
- ์์คํ์ ์ค๋นํ๋ ๊ณผ์ (๊ฐ์ฒด ์์ฑ, ์์กด์ฑ ์ฐ๊ฒฐ ๋ฑ)๊ณผ ์ค๋น ๊ณผ์  ์ดํ์ ์ด์ด์ง๋ ๋ฐํ์ ๋ก์ง(๋๋ฉ์ธ)์ ๋ถ๋ฆฌํด์ผ ํ๋ค.
  - ๋ฐฉ๋ฒ
    - ์์ฑ๊ณผ ๊ด๋ จ๋ ์ฝ๋๋ main์ด๋ main์ด ํธ์ถํ๋ ๋ชจ๋๋ก ์ฎ๊ธด๋ค.
    - ๋ฐํ์ ๋ก์ง ๋์ค์ ๊ฐ์ฒด๋ฅผ ์์ฑํด์ผ ํ๋ ๊ฒฝ์ฐ ABSTRACT FACTORY ํจํด์ ์ฌ์ฉํ๋ค.
    - ์์กด์ฑ ์ฃผ์(DI)
- ๊ด์ฌ์ฌ๋ฅผ ๋ถ๋ฆฌํ๋ผ.
  - ์๋ฐ์์ ์ฌ์ฉํ๋ ๊ด์  ํน์ ๊ด์ ๊ณผ ์ ์ฌํ ๋ฉ์ปค๋์ฆ
    - ์๋ฐ ํ๋ก์
    - ์์ ์๋ฐ AOP ํ๋ ์์ํฌ
    - AspectJ

## ๐ ๋ชฉ์ฐจ

- [01. ๋์๋ฅผ ์ธ์ด๋ค๋ฉด?](#01-๋์๋ฅผ-์ธ์ด๋ค๋ฉด)
- [02. ์์คํ ์ ์๊ณผ ์์คํ ์ฌ์ฉ์ ๋ถ๋ฆฌํ๋ผ](#02-์์คํ-์ ์๊ณผ-์์คํ-์ฌ์ฉ์-๋ถ๋ฆฌํ๋ผ)
- [03. ํ์ฅ](#03-ํ์ฅ)
- [04. ์๋ฐ ํ๋ก์](#04-์๋ฐ-ํ๋ก์)
- [05. ์์ ์๋ฐ AOP ํ๋ ์์ํฌ](#05-์์-์๋ฐ-aop-ํ๋ ์์ํฌ)
- [06. AspectJ ๊ด์ ](#06-aspectj-๊ด์ )
- [07. ํ์คํธ ์ฃผ๋ ์์คํ ์ํคํ์ฒ ๊ตฌ์ถ](#07-ํ์คํธ-์ฃผ๋-์์คํ-์ํคํ์ฒ-๊ตฌ์ถ)
- [08. ์์ฌ ๊ฒฐ์ ์ ์ต์ ํํ๋ผ](#08-์์ฌ-๊ฒฐ์ ์-์ต์ ํํ๋ผ)
- [09. ๋ช๋ฐฑํ ๊ฐ์น๊ฐ ์์ ๋ ํ์ค์ ํ๋ชํ๊ฒ ์ฌ์ฉํ๋ผ](#09-๋ช๋ฐฑํ-๊ฐ์น๊ฐ-์์-๋-ํ์ค์-ํ๋ชํ๊ฒ-์ฌ์ฉํ๋ผ)
- [10. ์์คํ์ ๋๋ฉ์ธ ํนํ ์ธ์ด๊ฐ ํ์ํ๋ค](#10-์์คํ์-๋๋ฉ์ธ-ํนํ-์ธ์ด๊ฐ-ํ์ํ๋ค)
- [11. ๊ฒฐ๋ก ](#11-๊ฒฐ๋ก )

## 01. ๋์๋ฅผ ์ธ์ด๋ค๋ฉด?

- ๋์๊ฐ ์ ๋์๊ฐ๋ ์ด์ ๋
    1. ๋์์๋ ํฐ ๊ทธ๋ฆผ์ ๊ทธ๋ฆฌ๋ ์ฌ๋๋ค๋ ์์ผ๋ฉฐ ์์ ์ฌํญ์ ์ง์คํ๋ ์ฌ๋๋ค๋ ์๋ค.
    2. ์ ์ ํ ์ถ์ํ์ ๋ชจ๋ํ ๋๋ฌธ์ด๋ค. ๊ทธ๋์ ํฐ ๊ทธ๋ฆผ์ ์ดํดํ์ง ๋ชปํ ์ง๋ผ๋ ๊ฐ์ธ๊ณผ ๊ฐ์ธ์ด ๊ด๋ฆฌํ๋ '๊ตฌ์ฑ์์'๋ ํจ์จ์ ์ผ๋ก ๋์๊ฐ๋ค.
- ์ํํธ์จ์ด ํ๋ ๋์์ฒ๋ผ ๊ตฌ์ํ๋ค. :white_check_mark: ๊นจ๋ํ ์ฝ๋๋ฅผ ๊ตฌํํ๋ฉด ๋ฎ์ ์ถ์ํ ์์ค์์ ๊ด์ฌ์ฌ๋ฅผ ๋ถ๋ฆฌํ๊ธฐ ์ฌ์์ง๋ค.
- ์ด ์ฅ์์๋ ๋์ ์ถ์ํ ์์ค, ์ฆ ์์คํ ์์ค์์๋ ๊นจ๋ํจ์ ์ ์งํ๋ ๋ฐฉ๋ฒ์ ์ดํด๋ณธ๋ค. :point_right: ์๋ก 

## 02. ์์คํ ์ ์๊ณผ ์์คํ ์ฌ์ฉ์ ๋ถ๋ฆฌํ๋ผ

- ์ ์(construction)์ ์ฌ์ฉ(use)์ ์์ฃผ ๋ค๋ฅด๋ค.
- :white_check_mark: ์ํํธ์จ์ด ์์คํ์ (์ ํ๋ฆฌ์ผ์ด์ ๊ฐ์ฒด๋ฅผ ์ ์ํ๊ณ  ์์กด์ฑ์ ์๋ก '์ฐ๊ฒฐ'ํ๋) ์ค๋น ๊ณผ์ ๊ณผ (์ค๋น ๊ณผ์  ์ดํ์ ์ด์ด์ง๋) ๋ฐํ์ ๋ก์ง์ ๋ถ๋ฆฌํด์ผ ํ๋ค.
- ์์ ๋จ๊ณ๋ ๋ชจ๋  ์ ํ๋ฆฌ์ผ์ด์์ด ํ์ด์ผ ํ  ๊ด์ฌ์ฌ(concern)๋ค. **๊ด์ฌ์ฌ ๋ถ๋ฆฌ**๋ ์ฐ๋ฆฌ ๋ถ์ผ์์ ๊ฐ์ฅ ์ค๋๋๊ณ  ๊ฐ์ฅ ์ค์ํ ์ค๊ณ ๊ธฐ๋ฒ ์ค ํ๋๋ค.

#### ๋์ ์

```java
// ์ด๊ธฐํ ์ง์ฐ ํน์ ๊ณ์ฐ ์ง์ฐ ๊ธฐ๋ฒ
public Service getService() {
    if (service == null) {
        service = new MyServiceImpl(...);
    }
    return service;
}
```

์ด๊ธฐํ ์ง์ฐ(Lazy Initialization) ํน์ ๊ณ์ฐ ์ง์ฐ(Lazy Evaluation) ๊ธฐ๋ฒ

- ์ฅ์ 
  - ์ค์ ๋ก ํ์ํ  ๋๊น์ง ๊ฐ์ฒด๋ฅผ ์์ฑํ์ง ์์ผ๋ฏ๋ก ๋ถํ์ํ ๋ถํ๊ฐ ๊ฑธ๋ฆฌ์ง ์๋๋ค. ๋ฐ๋ผ์ ์ ํ๋ฆฌ์ผ์ด์์ด ์์ํ๋ ์๊ฐ์ด ๊ทธ๋งํผ ๋นจ๋ผ์ง๋ค.
  - ์ด๋ค ๊ฒฝ์ฐ์๋ `null`์ ๋ฐํํ์ง ์๋๋ค.
- ๋ฌธ์ ์ 

  - `getService()` ๋ฉ์๋๊ฐ `MyServiceImpl`๊ณผ ์์ฑ์ ์ธ์์ ๋ช์์ ์ผ๋ก ์์กดํ๋ค. ๋ฐํ์ ๋ก์ง์์ ํด๋น ๊ฐ์ฒด๋ฅผ ์ ํ ์ฌ์ฉํ์ง ์๋๋ผ๋ ์์กด์ฑ์ ํด๊ฒฐํ์ง ์์ผ๋ฉด ์ปดํ์ผ์ด ์ ๋๋ค.
  - `MyServiceImpl`์ด ๋ฌด๊ฑฐ์ด ๊ฐ์ฒด๋ผ๋ฉด ์ ์ ํ ํ์คํธ ์ ์ฉ ๊ฐ์ฒด(TEST DOUBLE ์ด๋ MOCK OBJECT)๋ฅผ service ํ๋์ ํ ๋นํด์ผ ํ๋ค. ๋ํ, ์ผ๋ฐ ๋ฐํ์ ๋ก์ง์๋ค ๊ฐ์ฒด ์์ฑ ๋ก์ง์ ์์ด ๋์ ํ์ service๊ฐ `null`์ธ ๊ฒฝ๋ก์ `null`์ด ์๋ ๊ฒฝ๋ก ๋ฑ ๋ชจ๋  ์คํ ๊ฒฝ๋ก๋ ํ์คํธํด์ผ ํ๋ค.
  - ๋ฌด์๋ณด๋ค `MyServiceImpl`์ด ๋ชจ๋  ์ํฉ์ ์ ํฉํ ๊ฐ์ฒด์ธ์ง ๋ชจ๋ฅธ๋ค.

    > ๋ชจ๋  ์ํฉ์ ์ ํฉํ ๊ฐ์ฒด๋ผ๋ฉด ํด๋น ๋ฉ์๋์ ํด๋์ค๊ฐ ์ ์ฒด ๋ฌธ๋งฅ์ ์์์ผ ํ๋ค๋ ๊ฒ์ ์๋ฏธํ๋ค. ์ด๋ ๋ถ๊ฐ๋ฅํ๋ค. ๋ํ ๋ชจ๋  ๋ฌธ๋งฅ์ ์ ํฉํ  ๊ฐ๋ฅ์ฑ๋ ์ ๋ค.

### (1) Main

- :white_check_mark: ์์ฑ๊ณผ ๊ด๋ จ๋ ์ฝ๋๋ ๋ชจ๋ main์ด๋ main์ด ํธ์ถํ๋ ๋ชจ๋๋ก ์ฎ๊ธฐ๊ณ , ๋๋จธ์ง ์์คํ์ ๋ชจ๋  ๊ฐ์ฒด๊ฐ ์์ฑ๋์๊ณ  ๋ชจ๋  ์์กด์ฑ์ด ์ฐ๊ฒฐ๋์๋ค๊ณ  ๊ฐ์ ํ๋ค.
- main ํจ์์์ ์์คํ์ ํ์ํ ๊ฐ์ฒด๋ฅผ ์์ฑํ ํ ์ด๋ฅผ ์ ํ๋ฆฌ์ผ์ด์์ ๋๊ธด๋ค. ์ ํ๋ฆฌ์ผ์ด์์ ๊ทธ์  ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ  ๋ฟ์ด๋ค.
- ์ ํ๋ฆฌ์ผ์ด์์ main์ด๋ ๊ฐ์ฒด๊ฐ ์์ฑ๋๋ ๊ณผ์ ์ ์ ํ ๋ชจ๋ฅธ๋ค. ๋จ์ง ๋ชจ๋  ๊ฐ์ฒด๊ฐ ์ ์ ํ ์์ฑ๋์๋ค๊ณ  ๊ฐ์ ํ๋ค.

### (2) ํฉํ ๋ฆฌ

- :white_check_mark: ๋ฌผ๋ก  ๋๋ก๋ ๊ฐ์ฒด๊ฐ ์์ฑ๋๋ ์์ ์ ์ ํ๋ฆฌ์ผ์ด์์ด ๊ฒฐ์ ํ  ํ์๋ ์๊ธด๋ค. ์ด๋๋ ABSTRACT FACTORY ํจํด์ ์ฌ์ฉํ๋ค.

### (3) ์์กด์ฑ ์ฃผ์

- ์ฌ์ฉ๊ณผ ์ ์์ ๋ถ๋ฆฌํ๋ ๊ฐ๋ ฅํ ๋ฉ์ปค๋์ฆ ํ๋๊ฐ **์์กด์ฑ ์ฃผ์(DI)** ์ด๋ค. ์์กด์ฑ ์ฃผ์์ ์ ์ด ์ญ์ (IoC) ๊ธฐ๋ฒ์ ์์กด์ฑ ๊ด๋ฆฌ์ ์ ์ฉํ ๋ฉ์ปค๋์ฆ์ด๋ค.
- ์ ์ด ์ญ์ ์์๋ ํ ๊ฐ์ฒด๊ฐ ๋งก์ ๋ณด์กฐ ์ฑ์์ ์๋ก์ด ๊ฐ์ฒด์๊ฒ ์ ์ ์ผ๋ก ๋ ๋๊ธด๋ค. ์๋ก์ด ๊ฐ์ฒด๋ ๋๊ฒจ๋ฐ์ ์ฑ์๋ง ๋งก์ผ๋ฏ๋ก SRP๋ฅผ ์งํค๊ฒ ๋๋ค.
- ์์กด์ฑ ๊ด๋ฆฌ ๋งฅ๋ฝ์์ ๊ฐ์ฒด๋ ์์กด์ฑ ์์ฒด๋ฅผ ์ธ์คํด์ค๋ก ๋ง๋๋ ์ฑ์์ ์ง์ง ์๋๋ค. ๋์ ์ ์ด๋ฐ ์ฑ์์ ๋ค๋ฅธ '์ ๋ด' ๋ฉ์ปค๋์ฆ์ ๋๊ฒจ์ผ ํ๋ค. ๊ทธ๋ ๊ฒ ํจ์ผ๋ก์จ ์ ์ด๋ฅผ ์ญ์ ํ๋ค. ๋๊ฐ <u>'์ฑ์์ง' ๋งค์ปค๋์ฆ์ผ๋ก 'main' ๋ฃจํด์ด๋ ํน์ ์ปจํ์ด๋๋ฅผ ์ฌ์ฉํ๋ค.</u>

#### ์์

JNDI ๊ฒ์

```java
MyService myService = (MyService)(jndiContext.lookup("NameOfMyService"));
```

- ํธ์ถํ๋ ๊ฐ์ฒด๋ ์ค์ ๋ก ๋ฐํ๋๋ ๊ฐ์ฒด์ ์ ํ์ ์ ์ดํ์ง ์๋๋ค. ๋์  ํธ์ถํ๋ ๊ฐ์ฒด๋ ์์กด์ฑ์ ๋ฅ๋์ ์ผ๋ก ํด๊ฒฐํ๋ค.

> - "_ํธ์ถํ๋ ๊ฐ์ฒด_ " -> MyService ๋ผ๋ ์ธํฐํ์ด์ค ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ๋ ๊ฐ์ฒด
> - "_๊ฐ์ฒด์ ์ ํ์ ์ ์ดํ์ง ์๋๋ค._ " -> ๊ฐ์ฒด์ ์ ํ์ ๊ฐ์ฒด๋ฅผ ์์ฑํ ๊ณณ์์ ์ค์ ํ๋๋ก ์ฌ์ฉํ๋ค.
> - "_์์กด์ฑ์ ๋ฅ๋์ ์ผ๋ก ํด๊ฒฐํ๋ค._ " -> ์ด๋ฆ์ ํตํด์ ์์ฑํ ๊ฐ์ฒด๋ฅผ ์ฃผ์๋ฐ์ ํด๊ฒฐํ๋ค.

์ง์ ํ ์์กด์ฑ ์ฃผ์์ด๋?

- ํด๋์ค๊ฐ ์์กด์ฑ์ ํด๊ฒฐํ๋ ค ์๋ํ์ง ์๋๋ค. ํด๋์ค๋ ์์ ํ ์๋์ ์ด๋ค. ๋์ ์ ์์กด์ฑ์ ์ฃผ์ํ๋ ๋ฐฉ๋ฒ์ผ๋ก setter ๋ฉ์๋๋ ์์ฑ์ ์ธ์๋ฅผ ์ ๊ณตํ๋ค.
- DI ์ปจํ์ด๋๋ (๋๊ฐ ์์ฒญ์ด ๋ค์ด์ฌ ๋๋ง๋ค) ํ์ํ ๊ฐ์ฒด์ ์ธ์คํด์ค๋ฅผ ๋ง๋  ํ ์์ฑ์ ์ธ์๋ setter ๋ฉ์๋๋ฅผ ์ฌ์ฉํด ์์กด์ฑ์ ์ค์ ํ๋ค. ์ค์ ๋ก ์์ฑ๋๋ ๊ฐ์ฒด ์ ํ์ ์ค์  ํ์ผ์์ ์ง์ ํ๊ฑฐ๋ ํน์ ์์ฑ ๋ชจ๋์์ ์ฝ๋๋ฅผ ๋ช์ํ๋ค. ์) ์คํ๋ง ํ๋ ์์ํฌ

์ด๊ธฐํ ์ง์ฐ

- ๋๋ค์ DI ์ปจํ์ด๋๋ ํ์ํ  ๋๊น์ง๋ ๊ฐ์ฒด๋ฅผ ์์ฑํ์ง ์๊ณ , ๋๋ถ๋ถ์ ๊ณ์ฐ ์ง์ฐ์ด๋ ๋น์ทํ ์ต์ ํ์ ์ธ ์ ์๋๋ก ํฉํ ๋ฆฌ๋ฅผ ํธ์ถํ๊ฑฐ๋ ํ๋ก์๋ฅผ ์์ฑํ๋ ๋ฐฉ๋ฒ์ ์ ๊ณตํ๋ค.
- ์ฆ, ๊ณ์ฐ ์ง์ฐ ๊ธฐ๋ฒ์ด๋ ์ด์ ์ ์ฌํ ์ต์ ํ ๊ธฐ๋ฒ์์ ์ด๋ฐ ๋ฉ์ปค๋์ฆ์ ์ฌ์ฉํ  ์ ์๋ค.

## 03. ํ์ฅ

- ์ฐ๋ฆฌ๋ ์ค๋ ์ฃผ์ด์ง ์ฌ์ฉ์ ์คํ ๋ฆฌ์ ๋ง์ถฐ ์์คํ์ ๊ตฌํํด์ผ ํ๋ค. ๋ด์ผ์ ์๋ก์ด ์คํ ๋ฆฌ์ ๋ง์ถฐ ์์คํ์ ์กฐ์ ํ๊ณ  ํ์ฅํ๋ฉด ๋๋ค. (์ ์์ผ ๋ฐฉ์)
- TDD, ๋ฆฌํฉํฐ๋ง, ๊ทธ๋ฆฌ๊ณ  TDD์ ๋ฆฌํฉํฐ๋ง์ผ๋ก ์ป์ด์ง๋ ๊นจ๋ํ ์ฝ๋๋ ์ฝ๋ ์์ค์์ ์์คํ์ ์กฐ์ ํ๊ณ  ํ์ฅํ๊ธฐ ์ฝ๊ฒ ๋ง๋ ๋ค. :point_right: ๊นจ๋ํ ์ฝ๋ & ์ํคํ์ฒ์ ์ค์์ฑ
- ๊ด์ฌ์ฌ๋ฅผ ์ ์ ํ ๋ถ๋ฆฌํด ๊ด๋ฆฌํ๋ค๋ฉด ์ํํธ์จ์ด ์ํคํ์ฒ๋ ์ ์ง์ ์ผ๋ก ๋ฐ์ ํ  ์ ์๋ค. :point_right: ๊ด์ฌ์ฌ๋ฅผ ๋ถ๋ฆฌํ๋ ๊ฒ์ด ํต์ฌ์ด๋ค!

### ํก๋จ(cross-cutting) ๊ด์ฌ์ฌ

- ํ์ค์ ์ผ๋ก๋ ์์์ฑ ๋ฐฉ์์ ๊ตฌํํ ์ฝ๋๋ ์จ๊ฐ ๊ฐ์ฒด๋ก ํฉ์ด์ง๋ค. :point_right: ์์์ฑ ํ๋ ์์ํฌ ์์ญ๊ณผ ๋๋ฉ์ธ ์์ญ์ด ์ธ๋ฐํ ๋จ์๋ก ๊ฒน์น๋ค.
- **AOP(๊ด์ ์งํฅ ํ๋ก๊ทธ๋๋ฐ)** ๋ ํก๋จ ๊ด์ฌ์ฌ์ ๋์ฒํด ๋ชจ๋์ฑ์ ํ๋ณดํ๋ ์ผ๋ฐ์ ์ธ ๋ฐฉ๋ฒ๋ก ์ด๋ค.
- AOP์์ ๊ด์ ์ด๋ผ๋ ๋ชจ๋ ๊ตฌ์ฑ ๊ฐ๋์ "ํน์  ๊ด์ฌ์ฌ๋ฅผ ์ง์ํ๋ ค๋ฉด ์์คํ์์ ํน์  ์ง์ ๋ค์ด ๋์ํ๋ ๋ฐฉ์์ ์ผ๊ด์ฑ ์๊ฒ ๋ฐ๊ฟ์ผ ํ๋ค."๋ผ๊ณ  ๋ช์ํ๋ค. ๋ช์๋ ๊ฐ๊ฒฐํ ์ ์ธ์ด๋ ํ๋ก๊ทธ๋จ๋ฐ ๋ฉ์ปค๋์ฆ์ผ๋ก ์ํํ๋ค.

## 04. ์๋ฐ ํ๋ก์

์์

```java
public interface Bank {
    Collection<Account> getAccounts();
    void setAccounts(Collection<Account> accounts);
}

public class BankImpl implements Bank {
    private List<Account> accounts;

    Collection<Account> getAccounts() {
        return accounts;
    }

    void setAccounts(Collection<Account> accounts) {
        this.accounts = accounts;
    }
}

public class BankProxyHandler implements InvocationHandler {
    @Overrice
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // ๋ฐ์ดํฐ๋ฒ ์ด์ค์์ ๊ณ์ข ์ ๋ณด๋ฅผ ์ฝ์ด์์ Bank ๋ฉ์๋๋ฅผ ๊ตฌํํ๋ค.
    }
}
```

```java
Bank bank = (Bank) Proxy.newProxyInstance(
    Bank.class.getClassLoader(),
    new Class[] { Bank.class },
    new BankProxyHandler(new BankImpl())
);
```

๋จ์ 

- ์ฝ๋ '์'๊ณผ ํฌ๊ธฐ๋ ํ๋ก์์ ๋ ๊ฐ์ง ๋จ์ ์ด๋ค. ์ฆ, ํ๋ก์๋ฅผ ์ฌ์ฉํ๋ฉด ๊นจ๋ํ ์ฝ๋๋ฅผ ์์ฑํ๊ธฐ ์ด๋ ต๋ค.
- ๋ํ, ์์คํ ๋จ์๋ก ์คํ '์ง์ '์ ๋ช์ํ๋ ๋ฉ์ปค๋์ฆ๋ ์ ๊ณตํ์ง ์๋๋ค.

## 05. ์์ ์๋ฐ AOP ํ๋ ์์ํฌ

- ์์ ์๋ฐ ๊ด์ ์ ๊ตฌํํ๋ ์คํ๋ง AOP, JBoss AOP ๋ฑ๊ณผ ๊ฐ์ ์ฌ๋ฌ ์๋ฐ ํ๋ ์์ํฌ๋ ๋ด๋ถ์ ์ผ๋ก ํ๋ก์๋ฅผ ์ฌ์ฉํ๋ค.
- ์คํ๋ง์ ๋น์ฆ๋์ค ๋ผ๋ฆฌ๋ฅผ POJO๋ก ๊ตฌํํ๋ค.
  - POJO๋ ์์ํ๊ฒ ๋๋ฉ์ธ์ ์ด์ ์ ๋ง์ถ๋ค. POJO๋ ์ํฐํ๋ผ์ด์ฆ ํ๋ ์์ํฌ์ (๊ทธ๋ฆฌ๊ณ  ๋ค๋ฅธ ๋๋ฉ์ธ์๋) ์์กดํ์ง ์๋๋ค. ๋ฐ๋ผ์ ํ์คํธ๊ฐ ๊ฐ๋์ ์ผ๋ก ๋ ์ฝ๊ณ  ๊ฐ๋จํ๋ค. (<-> EJB)
  - ์๋์ ์ผ๋ก ๋จ์ํ๊ธฐ ๋๋ฌธ์ ์ฌ์ฉ์ ์คํ ๋ฆฌ๋ฅผ ์ฌ๋ฐ๋ก ๊ตฌํํ๊ธฐ ์ฌ์ฐ๋ฉฐ ๋ฏธ๋ ์คํ ๋ฆฌ์ ๋ง์ถฐ ์ฝ๋๋ฅผ ๋ณด์ํ๊ณ  ๊ฐ์ ํ๊ธฐ ํธํ๋ค.
- ํ๋ก๊ทธ๋๋จธ๋ ์ค์  ํ์ผ์ด๋ API๋ฅผ ์ฌ์ฉํด ํ์์ ์ธ ์ ํ๋ฆฌ์ผ์ด์ ๊ธฐ๋ฐ ๊ตฌ์กฐ๋ฅผ ๊ตฌํํ๋ค. ์์์ฑ, ํธ๋์ญ์, ๋ณด์, ์บ์, ์ฅ์ ์กฐ์น ๋ฑ๊ณผ ๊ฐ์ ํก๋จ ๊ด์ฌ์ฌ๋ ํฌํจ๋๋ค.
- ์ค์ ๋ก ์คํ๋ง์ด๋ JBoss ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ ๊ด์ ์ ๋ช์ํ๋ค. ์ด๋ ํ๋ ์์ํฌ๋ ์ฌ์ฉ์๊ฐ ๋ชจ๋ฅด๊ฒ ํ๋ก์๋ ๋ฐ์ดํธ์ฝ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํด ์ด๋ฅผ ๊ตฌํํ๋ค. ์ด๋ฐ ์ ์ธ๋ค์ด ์์ฒญ์ ๋ฐ๋ผ ์ฃผ์ ๊ฐ์ฒด๋ฅผ ์์ฑํ๊ณ  ์๋ก ์ฐ๊ฒฐํ๋ ๋ฑ DI ์ปจํ์ด๋์ ๊ตฌ์ฒด์ ์ธ ๋์์ ์ ์ดํ๋ค.
- ์์ ์์์์ ํด๋ผ์ด์ธํธ๋ `Bank` ๊ฐ์ฒด์์ `getAccounts()`๋ฅผ ํธ์ถํ๋ค๊ณ  ๋ฏฟ์ง๋ง ์ค์ ๋ก๋ `Bank` POJO์ ๊ธฐ๋ณธ ๋์์ ํ์ฅํ ์ค์ฒฉ DECORATOR ๊ฐ์ฒด ์งํฉ์ ๊ฐ์ฅ ์ธ๊ณฝ๊ณผ ํต์ ํ๋ค.

```xml
<!-- app.xml -->
<beans>
    <!-- ... -->
    <bean
        id="appDataSource"
        class="org.apache.commons.dbcp.BasicDataSource"
        destory-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="me"
    />
    <bean
        id="bankDataAccessObject"
        class="com.example.banking.persistence.BankDataAccessObject"
        p:dataSource-ref="appDataSource"
     />
    <bean
        id="bank"
        class="com.example.banking.model.Bank"
        p:dataAccessObject-ref="bankDataAccessObject"
    />
</beans>
```

```java
XmlBeanFactory bf = new XmlBeanFactory(new ClassPathResource("app.xml", getClass()));
Bank bank = (Bank) bf.getBean("bank");
```

- ์คํ๋ง ๊ด๋ จ ์๋ฐ ์ฝ๋๊ฐ ๊ฑฐ์ ํ์ ์์ผ๋ฏ๋ก ์ ํ๋ฆฌ์ผ์ด์์ ์ฌ์ค์ ์คํ๋ง๊ณผ ๋๋ฆฝ์ ์ด๋ค. :point_right: ์ฝ๋๋ฅผ ํ์คํธํ๊ณ  ๊ฐ์ ํ๊ณ  ๋ณด์ํ๊ธฐ ์ฉ์ดํด์ง๋ค.

## 06. AspectJ ๊ด์ 

- ๊ด์ฌ์ฌ๋ฅผ ๊ด์ ์ผ๋ก ๋ถ๋ฆฌํ๋ ๊ฐ์ฅ ๊ฐ๋ ฅํ ๋๊ตฌ๋ AspectJ ์ธ์ด์ด๋ค. AspectJ๋ ์ธ์ด ์ฐจ์์์ ๊ด์ ์ ๋ชจ๋ํ ๊ตฌ์ฑ์ผ๋ก ์ง์ํ๋ ์๋ฐ ์ธ์ด ํ์ฅ์ด๋ค.
- AspectJ๋ ๊ด์ ์ ๋ถ๋ฆฌํ๋ ๊ฐ๋ ฅํ๊ณ  ํ๋ถํ ๋๊ตฌ ์งํฉ์ ์ ๊ณตํ์ง๋ง, ์ ๋๊ตฌ๋ฅผ ์ฌ์ฉํ๊ณ  ์ ์ธ์ด ๋ฌธ๋ฒ๊ณผ ์ฌ์ฉ๋ฒ์ ์ตํ์ผ ํ๋ค๋ ๋จ์ ์ด ์๋ค.
- AspectJ '์ ๋ํ์ด์ ํผ'์ ์๋ก์ด ๋๊ตฌ์ ์๋ก์ด ์ธ์ด๋ผ๋ ๋ถ๋ด์ ์ด๋ ์ ๋ ์ํํ๋ค. '์ ๋ํ์ด์ ํผ'์ ์์ํ ์๋ฐ ์ฝ๋์ ์๋ฐ 5 ์ ๋ํ์ด์์ ์ฌ์ฉํด ๊ด์ ์ ์ ์ํ๋ค. ๋ํ, ์คํ๋ง ํ๋ ์์ํฌ๋ ์ ๋ํ์ด์ ๊ธฐ๋ฐ ๊ด์ ์ ์ฝ๊ฒ ์ฌ์ฉํ๋๋ก ๋ค์ํ ๊ธฐ๋ฅ์ ์ ๊ณตํ๋ค.

## 07. ํ์คํธ ์ฃผ๋ ์์คํ ์ํคํ์ฒ ๊ตฌ์ถ

- ์ ํ๋ฆฌ์ผ์ด์ ๋๋ฉ์ธ ๋ผ๋ฆฌ๋ฅผ POJO๋ก ์์ฑํ  ์ ์๋ค๋ฉด, ์ฆ ์ฝ๋ ์์ค์์ ์ํคํ์ฒ ๊ด์ฌ์ฌ๋ฅผ ๋ถ๋ฆฌํ  ์ ์๋ค๋ฉด, ์ง์ ํ ํ์คํธ ์ฃผ๋ ์ํคํ์ฒ ๊ตฌ์ถ์ด ๊ฐ๋ฅํด์ง๋ค. ๊ทธ๋๊ทธ๋ ์๋ก์ด ๊ธฐ์ ์ ์ฑํํด ๋จ์ํ ์ํคํ์ฒ๋ฅผ ๋ณต์กํ ์ํคํ์ฒ๋ก ํค์๊ฐ ์๋ ์๋ค.
- BDUF(Big Design Up Front)๋ฅผ ์ถ๊ตฌํ  ํ์๊ฐ ์๋ค.
  - BDUF๋ ๊ตฌํ์ ์์ํ๊ธฐ ์ ์ ์์ผ๋ก ๋ฒ์ด์ง ๋ชจ๋  ์ฌํญ์ ์ค๊ณํ๋ ๊ธฐ๋ฒ
  - ์ฒ์์ ์์ ๋ถ์ ๋ธ๋ ฅ์ ๋ฒ๋ฆฌ์ง ์์ผ๋ ค๋ ์ฌ๋ฆฌ์  ์ ํญ์ผ๋ก ์ธํด, ๊ทธ๋ฆฌ๊ณ  ์ฒ์ ์ ํํ ์ํคํ์ฒ๊ฐ ํฅํ ์ฌ๊ณ  ๋ฐฉ์์ ๋ฏธ์น๋ ์ํฅ์ผ๋ก ์ธํด, ๋ณ๊ฒฝ์ ์ฝ์ฌ๋ฆฌ ์์ฉํ์ง ๋ชปํ๊ธฐ ๋๋ฌธ์ ํด๋ก์ธ ์ ์๋ค.
  - ์ํํธ์จ์ด ๊ตฌ์กฐ๊ฐ ๊ด์ ์ ํจ๊ณผ์ ์ผ๋ก ๋ถ๋ฆฌํ๋ค๋ฉด, ๊ทน์ ์ธ ๋ณํ๊ฐ ๊ฒฝ์ ์ ์ผ๋ก ๊ฐ๋ฅํ๋ค.
- '์์ฃผ ๋จ์ํ๋ฉด์๋' ๋ฉ์ง๊ฒ ๋ถ๋ฆฌ๋ ์ํคํ์ฒ๋ก ๊ฒฐ๊ณผ๋ฌผ์ ๋น ๋ฅด๊ฒ ์ถ์ํ ํ, ๊ธฐ๋ฐ ๊ตฌ์กฐ๋ฅผ ์ถ๊ฐํ๋ฉฐ ์กฐ๊ธ์ฉ ํ์ฅํด ๋๊ฐ๋ ๊ด์ฐฎ๋ค. ์ธ๊ณ ์ต๋ ์น ์ฌ์ดํธ๋ค์ ๊ณ ๋์ ์๋ฃ ์บ์ฑ, ๋ณด์, ๊ฐ์ํ ๋ฑ์ ์ด์ฉํด ์์ฃผ ๋์ ๊ฐ์ฉ์ฑ๊ณผ ์ฑ๋ฅ์ ํจ์จ์ ์ด๊ณ ๋ ์ ์ฐํ๊ฒ ๋ฌ์ฑํ๋ค. ์ค๊ณ๊ฐ ์ต๋ํ ๋ถ๋ฆฌ๋์ด ๊ฐ ์ถ์ํ ์์ค๊ณผ ๋ฒ์์์ ์ฝ๋๊ฐ ์ ๋นํ ๋จ์ํ๊ธฐ ๋๋ฌธ์ด๋ค.
- :white_check_mark: ํ๋ก์ ํธ๋ฅผ ์์ํ  ๋๋ ์ผ๋ฐ์ ์ธ ๋ฒ์, ๋ชฉํ, ์ผ์ ์ ๋ฌผ๋ก ์ด๊ณ  ๊ฒฐ๊ณผ๋ก ๋ด๋์ ์์คํ์ ์ผ๋ฐ์ ์ธ ๊ตฌ์กฐ๋ ์๊ฐํด์ผ ํ๋ค. ํ์ง๋ง ๋ณํ๋ ํ๊ฒฝ์ ๋์ฒํด ์ง๋ก๋ฅผ ๋ณ๊ฒฝํ  ๋ฅ๋ ฅ๋ ๋ฐ๋์ ์ ์งํด์ผ ํ๋ค.
- ์ข์ API๋ ๊ฑธ๋ฆฌ์ ๊ฑฐ๋ฆฌ์ง ์์์ผ ํ๋ค. ๊ทธ๋์ผ ํ์ด ์ฐฝ์์ ์ธ ๋ธ๋ ฅ์ ์ฌ์ฉ์ ์คํ ๋ฆฌ์ ์ง์คํ๋ค. ๊ทธ๋ฆฌํ์ง ์์ผ๋ฉด ์ํคํ์ฒ์ ๋ฐ์ด ๋ฌถ์ฌ ๊ณ ๊ฐ์๊ฒ ์ต์ ์ ๊ฐ์น๋ฅผ ํจ์จ์ ์ผ๋ก ์ ๊ณตํ์ง ๋ชปํ๋ค.

### ์์ฝ

- ์ต์ ์ ์์คํ ๊ตฌ์กฐ๋ ๊ฐ๊ธฐ POJO (๋๋ ๋ค๋ฅธ) ๊ฐ์ฒด๋ก ๊ตฌํ๋๋ ๋ชจ๋ํ๋ ๊ด์ฌ์ฌ ์์ญ(๋๋ฉ์ธ)์ผ๋ก ๊ตฌ์ฑ๋๋ค.
- ์๋ก ๋ค๋ฅธ ์์ญ์ ํด๋น ์์ญ ์ฝ๋์ ์ต์ํ์ ์ํฅ์ ๋ฏธ์น๋ ๊ด์ ์ด๋ ์ ์ฌํ ๋๊ตฌ๋ฅผ ์ฌ์ฉํด ํตํฉํ๋ค.
- ํ์คํธ ์ฃผ๋ ๊ธฐ๋ฒ์ ์ ์ฉํ  ์ ์๋ค.

## 08. ์์ฌ ๊ฒฐ์ ์ ์ต์ ํํ๋ผ

- ๋ชจ๋์ ๋๋๊ณ  ๊ด์ฌ์ฌ๋ฅผ ๋ถ๋ฆฌํ๋ฉด ์ง์ฝ์ ์ธ ๊ด๋ฆฌ์ ๊ฒฐ์ ์ด ๊ฐ๋ฅํด์ง๋ค.
- ๊ฐ์ฅ์ ํฉํ ์ฌ๋์๊ฒ ์ฑ์์ ๋งก๊ธฐ๋ฉด ๊ฐ์ฅ ์ข๋ค. ์ต๋ํ ์ ๋ณด๋ฅผ ๋ชจ์ ์ต์ ์ ๊ฒฐ์ ์ ๋ด๋ฆฌ๊ธฐ ์ํด์๋ค.

### ์์ฝ

- ๊ด์ฌ์ฌ๋ฅผ ๋ชจ๋๋ก ๋ถ๋ฆฌํ POJO ์์คํ์ ๊ธฐ๋ฏผํจ์ ์ ๊ณตํ๋ค.
- ์ด๋ฐ ๊ธฐ๋ฏผํจ ๋ํ์ ์ต์  ์ ๋ณด์ ๊ธฐ๋ฐํด ์ต์ ์ ์์ ์ ์ต์ ์ ๊ฒฐ์ ์ ๋ด๋ฆฌ๊ธฐ๊ฐ ์ฌ์์ง๋ค.
- ๋ํ ๊ฒฐ์ ์ ๋ณต์ก์ฑ๋ ์ค์ด๋ ๋ค.

## 09. ๋ช๋ฐฑํ ๊ฐ์น๊ฐ ์์ ๋ ํ์ค์ ํ๋ชํ๊ฒ ์ฌ์ฉํ๋ผ

### ์์ฝ

- ํ์ค์ ์ฌ์ฉํ์ ๋์ ์ฅ์ 
  - ์์ด๋์ด์ ์ปดํฌ๋ํธ๋ฅผ ์ฌ์ฌ์ฉํ๊ธฐ ์ฝ๊ณ ,
  - ์ ์ ํ ๊ฒฝํ์ ๊ฐ์ง ์ฌ๋์ ๊ตฌํ๊ธฐ ์ฌ์ฐ๋ฉฐ,
  - ์ข์ ์์ด๋์ด๋ฅผ ์บก์ํํ๊ธฐ ์ฝ๊ณ ,
  - ์ปดํฌ๋ํธ๋ฅผ ์ฎ๊ธฐ ์ฝ๋ค.
- ํ์ค์ ์ฌ์ฉํ์ ๋์ ๋จ์ 
  - ํ์ค์ ๋ง๋๋ ์๊ฐ์ด ๋๋ฌด ์ค๋ ๊ฑธ๋ ค ์๊ณ๊ฐ ๊ธฐ๋ค๋ฆฌ์ง ๋ชปํ๋ค.

## 10. ์์คํ์ ๋๋ฉ์ธ ํนํ ์ธ์ด(DSL)๊ฐ ํ์ํ๋ค

- DSL(Domain-Specific Language)์ ๊ฐ๋จํ ์คํฌ๋ฆฝํธ ์ธ์ด๋ ํ์ค ์ธ์ด๋ก ๊ตฌํํ API๋ฅผ ๊ฐ๋ฆฌํจ๋ค.
- DSL๋ก ์ง  ์ฝ๋๋ ๋๋ฉ์ธ ์ ๋ฌธ๊ฐ๊ฐ ์์ฑํ ๊ตฌ์กฐ์ ์ธ ์ฐ๋ฌธ์ฒ๋ผ ์ฝํ๋ค.
- ์ข์ DSL์ ๋๋ฉ์ธ ๊ฐ๋๊ณผ ๊ทธ ๊ฐ๋์ ๊ตฌํํ ์ฝ๋ ์ฌ์ด์ ์กด์ฌํ๋ ์์ฌ์ํต ๊ฐ๊ทน์ ์ค์ฌ์ค๋ค. ๋๋ฉ์ธ ์ ๋ฌธ๊ฐ๊ฐ ์ฌ์ฉํ๋ ์ธ์ด๋ก ๋๋ฉ์ธ ๋ผ๋ฆฌ๋ฅผ ๊ตฌํํ๋ฉด ๋๋ฉ์ธ์ ์๋ชป ๊ตฌํํ  ๊ฐ๋ฅ์ฑ์ด ์ค์ด๋ ๋ค.
- ํจ๊ณผ์ ์ผ๋ก ์ฌ์ฉํ๋ค๋ฉด ์ถ์ํ ์์ค์ ์ฝ๋ ๊ด์ฉ๊ตฌ๋ ๋์์ธ ํจํด ์ด์์ผ๋ก ๋์ด์ฌ๋ฆฐ๋ค.

### ์์ฝ

- DSL์ ์ฌ์ฉํ๋ฉด ๊ณ ์ฐจ์ ์ ์ฑ์์ ์ ์ฐจ์ ์ธ๋ถ์ฌํญ์ ์ด๋ฅด๊ธฐ๊น์ง ๋ชจ๋  ์ถ์ํ ์์ค๊ณผ ๋ชจ๋  ๋๋ฉ์ธ์ POJO๋ก ํํํ  ์ ์๋ค.

## 11. ๊ฒฐ๋ก 

- ๊นจ๋ํ์ง ๋ชปํ ์ํคํ์ฒ๋
  - ๋๋ฉ์ธ ๋ผ๋ฆฌ๋ฅผ ํ๋ฆฌ๋ฉฐ ๊ธฐ๋ฏผ์ฑ์ ๋จ์ด๋จ๋ฆฐ๋ค. ๋๋ฉ์ธ ๋ผ๋ฆฌ๊ฐ ํ๋ ค์ง๋ฉด ์ ํ ํ์ง์ด ๋จ์ด์ง๋ค.
  - ๋ฒ๊ทธ๊ฐ ์จ์ด๋ค๊ธฐ ์ฌ์์ง๊ณ , ์คํ ๋ฆฌ๋ฅผ ๊ตฌํํ๊ธฐ ์ด๋ ค์์ง๋ค.
  - ๊ธฐ๋ฏผ์ฑ์ด ๋จ์ด์ง๋ฉด ์์ฐ์ฑ์ด ๋ฎ์์ ธ TDD๊ฐ ์ ๊ณตํ๋ ์ฅ์ ์ด ์ฌ๋ผ์ง๋ค.
- ๋ชจ๋  ์ถ์ํ ๋จ๊ณ์์ ์๋๋ ๋ชํํ ํํํด์ผ ํ๋ค. ๊ทธ๋ฌ๋ ค๋ฉด POJO๋ฅผ ์์ฑํ๊ณ  ๊ด์  ํน์ ๊ด์ ๊ณผ ์ ์ฌํ ๋ฉ์ปค๋์ฆ์ ์ฌ์ฉํด ๊ฐ ๊ตฌํ ๊ด์ฌ์ฌ๋ฅผ ๋ถ๋ฆฌํด์ผ ํ๋ค.
- ์์คํ์ ์ค๊ณํ๋  ๊ฐ๋ณ ๋ชจ๋์ ์ค๊ณํ๋ , ์ค์ ๋ก ๋์๊ฐ๋ ๊ฐ์ฅ ๋จ์ํ ์๋จ์ ์ฌ์ฉํด์ผ ํ๋ค.
