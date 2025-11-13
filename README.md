# Resolvendo o Erro gdk-pixbuf g_module_open specified module could not be found no Windows com Conda

### TL;DR
Se você está enfrentando erros como "The specified procedure could not found" ou "g_module_open() failed" ao executar gdk-pixbuf-query-loaders.exe no Windows com Miniconda, o problema é incompatibilidade de dependências entre gdk-pixbuf e gettext. A solução é usar o Mamba para criar um ambiente limpo, pois ele resolve dependências de forma mais inteligente.

#### Solução rápida
```shell
mamba create -n meu_ambiente python=3.11 gdk-pixbuf -c conda-forge
```

#### Problema
Ao configurar o ComfyUI ou outras aplicações que dependem do gdk-pixbuf no Windows 10/11 com Miniconda, você pode encontrar erros como:

```shell

conda install -c conda-forge gdk-pixbuf

Preparing transaction: done
Verifying transaction: done
Executing transaction: \ g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\libpixbufloader-svg.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\com
fyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\libpixbufloader-svg.dll': The specified module could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-ani.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-ani.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-bmp.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-bmp.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-gif.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-gif.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-icns.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf
-2.0\2.10.0\loaders\pixbufloader-icns.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-ico.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-ico.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-pnm.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-pnm.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-qtif.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf
-2.0\2.10.0\loaders\pixbufloader-qtif.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-tga.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-tga.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-tiff.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf
-2.0\2.10.0\loaders\pixbufloader-tiff.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-xbm.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-xbm.dll': The specified procedure could not be found.
g_module_open() failed for C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders\pixbufloader-xpm.dll: 'C:\Users\magno\AppData\Local\miniconda3\envs\comfyui\Library\lib\gdk-pixbuf-
2.0\2.10.0\loaders\pixbufloader-xpm.dll': The specified procedure could not be found.
```

```shell
(comfyui) PS C:\comfyui> & "$env:CONDA_PREFIX\Library\bin\gdk-pixbuf-query-loaders.exe"
g_module_open() failed for ...\pixbufloader-ani.dll: '...\pixbufloader-ani.dll': The specified procedure could not be found.
```

Ou pior, ao executar sem argumentos:

```shell
(comfyui) PS C:\comfyui> & "$env:CONDA_PREFIX\Library\bin\gdk-pixbuf-query-loaders.exe"
g_module_open() failed for ...\pixbufloader-ani.dll: '...\pixbufloader-ani.dll': The specified procedure could not be found.
```

E uma mensagem de popup:

```shell
"Não foi possível localizar o ponto de entrada do procedimento libintl_bind_textdomain_codeset na biblioteca de vínculo dinâmico gdk_pixbuf-2.0-0.dll"
```

<img width="444" height="201" alt="image" src="https://github.com/user-attachments/assets/ddf64785-7598-42bd-808c-46065d5493b3" />

#### Contexto
```
Sistema: Windows 10/11
Gerenciador: Miniconda3
Pacote problemático: gdk-pixbuf do canal conda-forge
Ambiente: ComfyUI, aplicações GTK, ou qualquer coisa que processe imagens
```

#### Diagnóstico
A mensagem "The specified procedure could not be found" (Windows error 127) indica que as DLLs dos plugins (pixbufloader-*.dll) estão tentando chamar funções que não existem na versão das dependências instaladas.
Causa raiz: A versão do gdk-pixbuf parece que foi compilada contra uma versão do gettext/libintl que não está presente no seu ambiente. Especificamente, a função libintl_bind_textdomain_codeset está ausente em libintl-8.dll.
Isso possivelmente acontece porque o solver de dependências do Conda (que usa libsolv em Python) pode escolher combinações de pacotes que teoricamente "satisfazem" as dependências, mas na prática são incompatíveis no Windows.


### Solução Passo-a-Passo
Método que funcionou: Usar Mamba ✅

#### 1. Instale o Mamba no ambiente base (se ainda não tiver)
```
conda activate base
conda install -n base -c conda-forge mamba
```

#### 2. Crie um NOVO ambiente limpo
```
mamba create -n nome-env python=3.11 -c conda-forge
conda activate nome-env
```

#### 3. Instale gdk-pixbuf (o Mamba resolve tudo)
```
mamba install -c conda-forge gdk-pixbuf
```

#### 4. TESTE (deve funcionar!)
```
& "$env:CONDA_PREFIX\Library\bin\gdk-pixbuf-query-loaders.exe"
```

```
(comfyui) C:\comfyui>gdk-pixbuf-query-loaders.exe
# GdkPixbuf Image Loader Modules file
# Automatically generated file, do not edit
# Created by gdk-pixbuf-query-loaders from gdk-pixbuf-2.44.4
#
# LoaderDir = C:\Users\magno.almeida\.local\share\mamba\envs\comfyui\Library\lib\gdk-pixbuf-2.0\2.10.0\loaders
#
"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\libpixbufloader_svg.dll"
"svg" 6 "gdk-pixbuf" "Scalable Vector Graphics" "LGPL"
"image/svg+xml" "image/svg" "image/svg-xml" "image/vnd.adobe.svg+xml" "text/xml-svg" "image/svg+xml-compressed" ""
"svg" "svgz" "svg.gz" ""
" <svg" "*    " 100
" <!DOCTYPE svg" "*             " 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-ani.dll"
"ani" 4 "gdk-pixbuf" "Windows animated cursor" "LGPL"
"application/x-navi-animation" ""
"ani" ""
"RIFF    ACON" "    xxxx    " 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-bmp.dll"
"bmp" 5 "gdk-pixbuf" "BMP" "LGPL"
"image/bmp" "image/x-bmp" "image/x-MS-bmp" ""
"bmp" ""
"BM" "" 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-gif.dll"
"gif" 4 "gdk-pixbuf" "GIF" "LGPL"
"image/gif" ""
"gif" ""
"GIF8" "" 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-icns.dll"
"icns" 4 "gdk-pixbuf" "MacOS X icon" "GPL"
"image/x-icns" ""
"icns" ""
"icns" "" 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-ico.dll"
"ico" 5 "gdk-pixbuf" "Windows icon" "LGPL"
"image/x-icon" "image/x-ico" "image/x-win-bitmap" "image/vnd.microsoft.icon" "application/ico" "image/ico" "image/icon" "text/ico" ""
"ico" "cur" ""
"  \001   " "zz znz" 100
"  \002   " "zz znz" 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-pnm.dll"
"pnm" 4 "gdk-pixbuf" "PNM/PBM/PGM/PPM" "LGPL"
"image/x-portable-anymap" "image/x-portable-bitmap" "image/x-portable-graymap" "image/x-portable-pixmap" ""
"pnm" "pbm" "pgm" "ppm" ""
"P1" "" 100
"P2" "" 100
"P3" "" 100
"P4" "" 100
"P5" "" 100
"P6" "" 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-qtif.dll"
"qtif" 4 "gdk-pixbuf" "QuickTime" "LGPL"
"image/x-quicktime" "image/qtif" ""
"qtif" "qif" ""
"abcdidsc" "xxxx    " 100
"abcdidat" "xxxx    " 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-tga.dll"
"tga" 4 "gdk-pixbuf" "Targa" "LGPL"
"image/x-tga" ""
"tga" "targa" ""
" \001\001" "x  " 100
" \001\t" "x  " 100
"  \002" "xz " 99
"  \003" "xz " 100
"  \n" "xz " 100
"  \v" "xz " 100

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-tiff.dll"
"tiff" 5 "gdk-pixbuf" "TIFF" "LGPL"
"image/tiff" ""
"tiff" "tif" ""
"MM *" "  z " 100
"II* " "   z" 100
"II* \020   CR\002 " "   z zzz   z" 0

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-xbm.dll"
"xbm" 4 "gdk-pixbuf" "XBM" "LGPL"
"image/x-xbitmap" ""
"xbm" ""
"#define " "" 100
"/*" "" 50

"lib\\gdk-pixbuf-2.0\\2.10.0\\loaders\\pixbufloader-xpm.dll"
"xpm" 4 "gdk-pixbuf" "XPM" "LGPL"
"image/x-xpixmap" ""
"xpm" ""
"/* XPM */" "" 100
```

#### O que é o Mamba e Por Que Funcionou?
##### Conda vs Mamba

| Feature             | Conda                | Mamba                       |
| ------------------- | -------------------- | --------------------------- |
| **Linguagem**       | Python               | C++                         |
| **Velocidade**      | 1x                   | **10-100x**                 |
| **Solver**          | libsolv (sequencial) | libmamba (paralelo)         |
| **Uso de RAM**      | Alto                 | **Baixo**                   |
| **Precisão**        | Boa                  | **Excelente**               |
| **Compatibilidade** | 100%                 | 99.9% (drop-in replacement) |

#### Por que funcionou no meu caso?
O Conda possivelmente escolheu uma build de gdk-pixbuf que foi compilada contra uma versão do gettext que não estava disponível no Windows. O solver do Mamba analisou todas as combinações possíveis e selecionou a build correta (2.44.4) com dependências 100% compatíveis.

#### Posso usar só o Mamba?
Sim, quase sempre. Duas estratégias:
1. Mambaforge: Distribuição completa que usa Mamba por padrão (substitui Miniconda)
2. Híbrido: Instale Mamba no base (conda install mamba) e use:
- mamba install/create → para resolver pacotes
- conda activate → para ativar ambientes

#### Quando usar Conda puro?
- conda build (criar pacotes)
- Ferramentas que exigem conda explicitamente
- Se encontrar bug raro no Mamba

#### Conclusão
##### Lições Aprendidas
- Erros de "procedure not found" em DLLs no Windows quase sempre indicam incompatibilidade de versão.
- O Conda nem sempre resolve dependências perfeitamente, especialmente para pacotes compilados (C/C++).
- Mamba é uma ferramenta essencial no ecossistema Conda, não apenas um "adorno".
- Sempre teste o gdk-pixbuf-query-loaders.exe após instalar.


#### Agradecimentos: 
Este guia foi baseado em um troubleshooting real. 
O erro foi resolvido com ajuda do Kimi V2. Todos os outros modelos, Grok 4, GPT 5, Claude 4.1, Gemini 2.5 persistiam em tentar resolver com o conda, mas nenhuma alternativa funcionou. Praticamente fiquei 2 semanas pesquisando e tentando alternativas, mas admito que não conhecia do mamba, com certeza teria tentado com ele antes de ter perdido tanto tempo. =/
