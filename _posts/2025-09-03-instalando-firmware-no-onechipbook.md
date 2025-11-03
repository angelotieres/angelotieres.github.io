---
title: Como instalar o firmware do OneChipBook
date: 2025-09-03 00:00:00 Z
layout: post
categorias:
- MSX
- Retrocomputação
---

Após dois longos meses de espera, finalmente chegou meu OneChipBook... ok, mas que raios é um OneChipBook?

<img src="/images/post01/MSXBOOK.webp" width="60%" height="60%">

Anteriormente chamado de MSXBook ~~cujo nome foi alterado por medo de receberem um processo do Nishi~~, o OneChipBook é um notebook que "emula" um MSX2+ em hardware através de um FPGA. Ponto. Simples assim. Mas vamos às specs...

- Tela de 9,7" com resolução 1024 x 768;
- FPGA Altera Cyclone EP1C12Q240;
- 32 Mb SDRAM;
- Bateria de 4.000 mAH com carregamento por USB-C;
- Teclado mecânico;
- Saídas de vídeo VGA, AV e S-Video;
- 1 slot de expansão externo (vulta entrada de cartucho);
- 1 slot de expansão interno;
- Auto-falantes ~~péssimos~~ com amplificador embutido e saida P2;
- 1 porta de Firmware (Active Serial Programming);
- 2 portas DB9 para joysticks;
- 1 porta de teclado PS2;
- 1 porta USB 2.0;
- 1 entrada de cartão SD.

Para saber mais a respeito ~~do que tenho paciência para informar~~, recomendo [esta postagem do Retrópolis](https://retropolis.com.br/2025/02/17/msxbook/).

Devido às patentes do MSX, o OneChipBook vêm sem firmware instalado, de forma que ao inicializá-lo você só verá um teste de display. No entanto, como ele foi totalmente baseado no OneChipMSX, é compatível com os firmwares do KdL, então é hora de instalar um.

## Instalando o Quartus II

A primeira coisa da qual se precisa é a aplicação Quartus II da Altera. Após instalar a versão grátis, rapidamente você se dará conta da necessidade das instalações adicionais, então prefira perder um pouco mais de tempo e baixar a ISO completa do que ficar baixando tudo aos poucos. Vou deixar o link da ISO do Quartus II 13.1 Web Edition (grátis) para Windows [aqui](https://mega.nz/file/3sAlxBDC#VAsw78GY_cS0EchSyzDgywGUo6y6EZLVlnuMUqm6agE), mas caso se encontre desatualizada, segue de qualquer forma o [link do download na Intel](https://www.intel.com/content/www/us/en/collections/products/fpga/software/downloads.html?s=Newest).

## Instalando o USB Blaster

A segunda necessidade é o driver do USB Blaster para conectar o FPGA ao computador, e é neste ponto que a brincadeira começa a ficar complexa, dada a dificuldade em encontrar o driver para download e a quantidade de erros possíveis, principalmente no Windows 11. Teoricamente o driver deveria vir junto à aplicação Quartus II.

<img src="/images/post01/post_02_img.png" width="40%" height="40%">

Mas na prática, você ganha um dispositivo não reconhecido de presente.

<img src="/images/post01/post_01_img.png" width="40%" height="40%">

Quanto a dificuldade em encontrar o driver, lá pela terceira página de busca do Google encontrei um que não viesse com um malware de brinde (crianças, não façam isso fora de sandbox ou VM isolada em casa!). Disponibilizei para download [aqui](/downloads/Usb_blaster.zip).

Como o checksum não bate (é raro, mas acontece muito!) você poderá ter problemas ao instalar no Windows. Se isso acontecer, o amaldiçoado código 39 surgirá:

<img src="/images/post01/post_04_img.png" width="70%" height="70%">

Há algumas formas diferentes para resolver esse erro, e uma delas deve funcionar para você (ou não):

### Alterando o registro do Windows

Um dos métodos é alterar o registro do Windows. Na barra de busca de seu Windows, digite `regedit` e procure por `HypervisorEnforcedCodeIntegrity`:

<img src="/images/post01/post_09_img.png" width="70%" height="70%">

Você deverá encontrar o valor como alguma configuração da HKEY_LOCAL_MACHINE. Troque o valor por zero, salve e reinicie o Windows.

<figure><img src="/images/post01/post_10_img.png" width="70%" height="70%"><figcaption>Encontrado na Setting2 do OperationalData...</figcaption></figure>

<figure><img src="/images/post01/post_11_img.png" width="70%" height="70%"><figcaption>...e configurado para zero</figcaption></figure>

Funcionando ou não, retorne a configuração do registro ao valor inicial após instalar o driver.

### Desabilitando a integridade da memória

Caso a opção acima não dê resultado (e após retornar o valor do registro ao original), a segunda opção é desabilitar a integridade da memória em Segurança do Windows > Segurança do dispositivo > Isolamento de núcleo. 

<img src="/images/post01/post_08_img.png" width="70%" height="70%">

O Windows lhe presenteará com uns quatro avisos diferentes de que você está ~~fazendo besteira~~ colocando seu computador em risco, então lembre-se de habilitar novamente após terminar a instalação do driver.

## Conectando a USB Blaster ao Quartus II

Considerando que uma das opções acima tenha funcionado, você finalmente alcançará a desejada tela de driver instalado. Caso contrário, ~~que Deus te ajude~~ tente desabilitar totalmente seu antivírus e firewall.

<img src="/images/post01/post_12_img.png" width="70%" height="70%">

O que você precisará agora é de um firmware compatível. Recomendo o firmware do MSX2+ do OneChipMSX [disponível aqui](https://gnogni.altervista.org/). Procure pelo OCM Pack em sua última versão. Extraia o conteúdo compactado e abra o software Quartus II com a USB Blaster conectada na USB.

<img src="/images/post01/post_13_img.png">

Vá em Programmer e altere ao lado direito para **Active Serial Programming**. 

<img src="/images/post01/post_14_img.png" width="70%" height="70%">

Se a USB Blaster aparecer disponível, vá para a instalação do firmware, caso contrário, clique em Hardware Setup > Add Hardware com a USB Blaster selecionada. Clique em Auto Detect e, se estiver tudo conforme a imagem abaixo, em Ok.

<img src="/images/post01/post_15_img.png" width="70%" height="70%">

## Instalando o firmware

Se tudo correr bem até aqui ~~será um milagre~~, clique em Add File para selecionar o arquivo .pof adequado, extraído do pacote baixado do [KdL Index](https://gnogni.altervista.org/).

<img src="/images/post01/post_16_img.png">

Com o OneChipBook ligado e a USB Blaster conectada nele e em seu computador, selecione todas as opções em **Program/Configure** e em **Verify**, dê Start e aguarde. O processo pode demorar um pouco.

<img src="/images/post01/post_18_img.png">`

Caso apareça a mensagem *failed* no canto direito, tente outro arquivo .pof ou reinicie o processo do zero. Se tudo der certo, aparecerá *Success* em verde e *voilà*, divirta-se (ou se irrite) com seu MSX!

Se quiser futucar o hardware, segue a [referência técnica para download](/downloads/1742232699212994.pdf).

<img src="/images/post01/tieresaurus12.png" width="70%" height="70%">
