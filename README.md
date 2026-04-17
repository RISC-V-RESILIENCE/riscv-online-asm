![visitors](https://visitor-badge.laobi.icu/badge?page_id=RISC-V-RESILIENCE.riscv-online-asm)
[![License: CC BY-SA 4.0](https://img.shields.io/badge/License-CC_BY--SA_4.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
![Language: Portuguese](https://img.shields.io/badge/Language-Portuguese-brightgreen.svg)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Prática-green)
![Status](https://img.shields.io/badge/Status-Educa%C3%A7%C3%A3o-brightgreen)
![Repository Size](https://img.shields.io/github/repo-size/RISC-V-RESILIENCE/RISC-V-Resilience-Workspace)
![Last Commit](https://img.shields.io/github/last-commit/RISC-V-RESILIENCE/RISC-V-Resilience-Workspace)

<!-- Animated Header -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1a56db,100:10b981&height=220&section=header&text=RISC-V%20Online%20Assembler&fontSize=42&fontColor=ffffff&animation=fadeIn&fontAlignY=35&desc=Assembler%20Online%20para%20RISC-V&descSize=18&descAlignY=55&descColor=94a3b8" width="100%" alt="RISC-V Online Assembler Header"/>
</p>

## RISC-V Online Assembler

Este é um fork do trabalho original de **Lucas Tesk**, um assembler online muito básico para assembly RISC-V (todas as variantes que o gas suporta).

### Objetivos deste Fork

Este repositório visa:
- **Ampliar a documentação** do projeto original
- **Adotar layout padronizado** com nossos outros projetos
- **Melhorar a experiência do usuário** com interface mais amigável
- **Adicionar novos recursos** educacionais para aprendizado de RISC-V

O projeto original usa uma versão compilada em webassembly do gnu as, objdump e objcopy para construir o assembly.

Foi feito REALMENTE rápido (provavelmente menos de 2h) para a série de Emulador RISC-V do Lucas Tesk (atualmente apenas em português em [https://www.youtube.com/playlist?list=PLEP_M2UAh9q6_2Jtvs9fgOVlRgsruii2m](https://www.youtube.com/playlist?list=PLEP_M2UAh9q6_2Jtvs9fgOVlRgsruii2m))

### Créditos

- **Autor Original:** Lucas Tesk
- **Fork e Expansão:** Projeto RISC-V Resilience

### Instalação do WebASM (Emscripten)

Para compilar os binutils para WebAssembly, você precisa instalar o Emscripten SDK primeiro.

#### Pré-requisitos

- Git
- Python 3.8+
- Node.js (recomendado para desenvolvimento)
- CMake (versão 3.15+)

#### Instalação do Emscripten SDK

```bash
# Clone o repositório do Emscripten SDK
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk

# Baixe e instale o SDK mais recente
./emsdk install latest
./emsdk activate latest

# Configure as variáveis de ambiente
source ./emsdk_env.sh

# Verifique a instalação
emcc --version
```

#### Configuração Permanente (Opcional)

Para tornar a configuração permanente, adicione ao seu `~/.bashrc` ou `~/.zshrc`:

```bash
# Adicione esta linha ao final do arquivo
source /caminho/para/emsdk/emsdk_env.sh
```

#### Verificação da Instalação

Após a instalação, verifique se tudo está funcionando corretamente:

```bash
# Teste a compilação de um programa simples
echo 'int main() { return 42; }' > test.c
emcc test.c -o test.html
node test.js  # Deve retornar 42
```

### Compiling binutils

```bash
wget http://ftp.gnu.org/gnu/binutils/binutils-2.31.tar.xz
tar -xf binutils-2.31.tar.xz
rm binutils-2.31.tar.xz
mkdir -p build
mkdir -p bins
mkdir -p web
cd build
source {PATH_TO_EMSDK}/emsdk_env.sh
echo "Module['FS'] = FS;" > post-js.txt
emconfigure ../binutils-2.31/configure --disable-doc --build=x86 --host=wasm32 --target=riscv64-linux-gnu
emmake make -j4 CFLAGS="-DHAVE_PSIGNAL=1 -DELIDE_CODE -D__GNU_LIBRARY__ -O2" LDFLAGS="-s MODULARIZE=1 -s FORCE_FILESYSTEM=1 --post-js $(pwd)/post-js.txt"
emmake make install DESTDIR="$(pwd)/../bins"
cp binutils/objcopy.wasm binutils/objdump.wasm gas/as-new.wasm ld/ld-new.wasm ../web
cd ..
cd bins
cp usr/local/bin/riscv64-linux-gnu-as ../web/riscv64-linux-gnu-as.js
cp usr/local/bin/riscv64-linux-gnu-objcopy ../web/riscv64-linux-gnu-objcopy.js
cp usr/local/bin/riscv64-linux-gnu-objdump ../web/riscv64-linux-gnu-objdump.js
cp usr/local/bin/riscv64-linux-gnu-objdump ../web/riscv64-linux-gnu-objdump.js
cp usr/local/bin/riscv64-linux-gnu-ld ../web/riscv64-linux-gnu-ld.js
```

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:10b981,50:1a56db,100:0f172a&height=120&section=footer" width="100%" alt="Footer"/>
</p>

---
**Resumo:** Este arquivo contém a documentação do assembler online RISC-V, incluindo instalação do WebASM e instruções de compilação dos binutils.
**Data de Criação:** 2025-10-15
**Autor:** Sistema de Documentação
**Versão:** 1.2
**Última Atualização:** 2025-04-17
**Atualizado por:** Sistema de Documentação
**Histórico de Alterações:**
- 2025-10-15 - Criado por Sistema de Documentação - Versão 1.0
- 2025-04-17 - Atualizado por Sistema de Documentação - Adicionado padrão de documentação com badges e headers - Versão 1.1
- 2025-04-17 - Atualizado por Sistema de Documentação - Adicionada seção de instalação do WebASM/Emscripten - Versão 1.2
