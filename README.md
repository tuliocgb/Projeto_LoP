# Projeto_LoP

//posicionamento do personagem
var x, y; 
// Variáveis para posições dos objetos 
var posX = [], posY = [], posX1 = [], posY1 = [], posX2 = [], posY2 = []; 

//Variáveis dos disparos
var xTiros = [], yTiros = [];  
var tirosAtivos = []; 
var qtTiros =  3;
var tempoTiro = -1;

//Variáveis para alterar o nível
var subirNivel = true;
var contador = 10;
  
//Variáveis das imagens
var imgCenario;
var imgJogador;
var imgInvasor1;
var imgInvasor2;
var imgInvasor3;
var imgExplosao1;

//Variável do som
var som;

//Váriável que indica a tela a ser exibida
var tela = 1;

//Informaçoes do jogo
var vidas =  3 ;
var pontos =  0 ; 
var nivel = 1;

function preload() {
  imgCenario = loadImage('img/espaco2.jpg');
  imgJogador = loadImage('img/nave.png');
  imgInvasor1 = loadImage('img/invasor1.png');
  imgInvasor2 = loadImage('img/invasor1.png');
  imgInvasor3 = loadImage('img/invasor1.png');
  imgTiro = loadImage('img/disparo.png');
  imgTela1 = loadImage('img/fundo.png');
  imgExplosao1 = loadImage('img/explosao.png');
  
  
  som = loadSound('mp3/som.mp3');

}

// Criação da tela de jogo
function setup() {
  som.setVolume(0.3);
  som.play();
  createCanvas(450, 600); 
  image(imgCenario);
// Objeto aleatório
  x = 175;
  y = 480;

  for ( i = 0; i < qtTiros; i++) {
    xTiros[i] = x;
    tirosAtivos[i] = false;  
    yTiros[i] = y; 
  }
  for(i=0;i<4;i++)
  {
    posX[i] = random(0,410); // Posição inicial no eixo x do objeto
    posY [i] = 0; // Posição inicial no eixo y do objeto
  }
  for(i=0;i<2;i++)
  {
    posX1 [i] = random(0,410); // Posição inicial no eixo x do objeto
    posY1 [i] = 0; // Posição inicial no eixo y do objeto
  }
  for(i=0;i<1;i++)
  {
    posX2 [i] = random(0,410); // Posição inicial no eixo x do objeto
    posY2 [i] = 0; // Posição inicial no eixo y do objeto
  }
  frameRate(50);
  
  
}

function draw() {
  

  if(tela==1){
    background(imgTela1);
    fill(255, 255, 255);
    textSize(50);
    textFont ("Times New Roman");
    text("A INVASÃO", 90, 240);
    textSize(20);
    text("Pressione ENTER para continuar...", 85, 280);

    if (keyIsDown(ENTER)){
      tela = 2;
    }
  }
  
  if(tela==2){
    clear();
    background(imgCenario);

  //Personagem do jogo
    image(imgJogador, x, y, 80, 120);
  

  //Movimentação do personagem utilizando o teclado
    if (keyIsDown(LEFT_ARROW)){
      if(x>5){
        x-=7;
      }
    }

    if (keyIsDown(RIGHT_ARROW)){
      if(x<(width-85)){
        x+=7;
      }
    }
  

    //Executando o disparo
    if ((keyIsDown(CONTROL)) && tempoTiro < 0) {
      tempoTiro = 5;   
  
      for ( i = 0; i < qtTiros; i++) {
        if ( tirosAtivos[i] ==  false ) {
          tirosAtivos[i] = true;
          xTiros[i] = x + 35;
          yTiros[i] = y;
          break; 
        
        } 
      } 
    }
    tempoTiro--;
    // movimentação do disparo
    for ( i = 0; i < qtTiros; i++) {
      if ( tirosAtivos[i] ) {
    // tiros 
        image(imgTiro, xTiros[i], yTiros[i], 15, 15);       
        yTiros[i] -= 5; 
        if (yTiros[i] < 0) {
          tirosAtivos[i] = false; 
        }
      } 
   
    }

    for(i=0; i<4; i++){ 
      image(imgInvasor1, posX[i], posY[i], 50, 50);
    }

  //Movimentação do invasor
    for(i=0;i<4;i++){
      posY[i] = posY[i] + 2;
    }
  
  

  

// Colisão do invasor1 com o personagem

    for(j=0;j<4;j++){
      if(dist( x, y , posX[j], posY[j]) <= 50){ // Para quando a posição do obstáculo for igual ao do jogador, jogador perde 1 vida
        vidas = vidas -1;
        posY[j] = -random(height);
        posX[j] = random(0,410);    
      }
    }

 // Colisão entre invasor1 e tiro 
  
      for(i=0; i < tirosAtivos.length ; i++){ 
        for(j=0; j<4; j++){
          if(dist(posX[j],  posY[j] , xTiros[i] , yTiros[i] ) <= 40) { //Para quando o obstáculo for atingido, apareça em oputra posição aleatória
       
            image(imgExplosao1, xTiros[i], yTiros[i], 50, 50);

            pontos = pontos + 1;
            tirosAtivos[i] = false;
            xTiros[i] = -1000; // Faz o tiro ir para uma posição que não aparece na tela
            yTiros[i] = -1000;
            posY[j] = -random(height);
            posX[j] = random(0,410); 
          }
        }
    }

    

    // Colisão do invasor2 com o personagem

    for(j=0;j<2;j++){
      if(dist( x, y , posX1[j], posY1[j]) <= 50){ // Para quando a posição do obstáculo for igual ao do jogador, jogador perde 1 vida
        vidas = vidas -1;
        posY1[j] = -random(height);
        posX1[j] = random(0,410);    
      }
    }

 // Colisão entre invasor2 e tiro 
  
      for(i=0; i < tirosAtivos.length ; i++){ 
        for(j=0; j<2; j++){
          if(dist(posX1[j],  posY1[j] , xTiros[i] , yTiros[i] ) <= 20) { //Para quando o obstáculo for atingido, apareça em oputra posição aleatória
       
            image(imgExplosao1, xTiros[i], yTiros[i], 25, 25);

            pontos = pontos + 2;
            tirosAtivos[i] = false;
            xTiros[i] = -1000; // Faz o tiro ir para uma posição que não aparece na tela
            yTiros[i] = -1000;
            posY1[j] = -random(height);
            posX1[j] = random(0,410); 
          }
        }
    }

    

    // Colisão do invasor3 com o personagem

    for(j=0;j<1;j++){
      if(dist( x, y , posX2[j], posY2[j]) <= 50){ // Para quando a posição do obstáculo for igual ao do jogador, jogador perde 1 vida
        vidas = vidas -1;
        posY2[j] = -random(height);
        posX2[j] = random(0,410);    
      }
    }

 // Colisão entre invasor3 e tiro 
  
      for(i=0; i < tirosAtivos.length ; i++){ 
        for(j=0; j<1; j++){
          if(dist(posX2[j],  posY2[j] , xTiros[i] , yTiros[i] ) <= 10) { //Para quando o obstáculo for atingido, apareça em oputra posição aleatória
       
            image(imgExplosao1, xTiros[i], yTiros[i], 15, 15);

            pontos = pontos + 3;
            tirosAtivos[i] = false;
            xTiros[i] = -1000; // Faz o tiro ir para uma posição que não aparece na tela
            yTiros[i] = -1000;
            posY2[j] = -random(height);
            posX2[j] = random(0,410); 
          }
        }
    }

 //Subindo de nível 
  
  if(pontos%10==0   && pontos!=0  && subirNivel == true){ 
    nivel = nivel + 1;
    subirNivel = false;
  }
  if(pontos==contador+1){
    subirNivel = true;
    contador+=10;
  }
  
  //Aumentando a dificuldade
  if(nivel == 2){
    //Velocidade do invaso1
    for(i=0;i<4;i++){
      posY[i] = posY[i] + 3;
    }
  }
  if(nivel == 3){
    for(i=0; i<2; i++){ 
      image(imgInvasor2, posX1[i], posY1[i], 30, 30);
    }
    //Velocidade do invasor1
    for(i=0;i<4;i++){
      posY[i] = posY[i] + 3;
    }    
    //Velocidade do invasor2
    for(i=0;i<2;i++){
      posY1[i] = posY1[i] + 5;
    }

  }
  if(nivel == 4){
    for(i=0; i<2; i++){ 
      image(imgInvasor2, posX1[i], posY1[i], 30, 30);
    }
    //Velocidade do invasor1
    for(i=0;i<4;i++){
      posY[i] = posY[i] + 4;
    }
    //Velocidade do invasor2
    for(i=0;i<2;i++){
      posY1[i] = posY1[i] + 7;
    }
  }
  if(nivel == 5){
    for(i=0; i<2; i++){ 
      image(imgInvasor2, posX1[i], posY1[i], 30, 30);
    }
    for(i=0; i<1; i++){ 
      image(imgInvasor3, posX2[i], posY2[i], 15, 15);
    }
    //Velocidade do invasor1
    for(i=0;i<4;i++){
      posY[i] = posY[i] + 4;
    }
    //Velocidade do invasor2
    for(i=0;i<2;i++){
      posY1[i] = posY1[i] + 7;
    }
    //Velocidade do invasor3
    for(i=0;i<1;i++){
      posY2[i] = posY2[i] + 11;
    }

  }

 
  //Exibindo informações na tela
    textSize ( 20 );
    textFont ("Times New Roman");
    stroke ( 135 , 206 , 235 );
    text ( " Vidas: " + vidas +  "  Pontos: " + pontos + "  Nível: " + nivel, 75 , 30 );
  
  // Se o invasor1 sair da tela, voltar lá pra cima
    for(i=0;i<4;i++){
      if (posY[i]>height){ 
        posY[i] = -random(height);
        posX[i] = random(0,410);
      }
    }
    // Se o invasor2 sair da tela, voltar lá pra cima
    for(i=0;i<2;i++){
      if (posY1[i]>height){ 
        posY1[i] = -random(height);
        posX1[i] = random(0,410);
      }
    }
    // Se o invasor3 sair da tela, voltar lá pra cima
    for(i=0;i<1;i++){
      if (posY2[i]>height){ 
        posY2[i] = -random(height);
        posX2[i] = random(0,410);
      }
    }
  
    for(i=0; i < tirosAtivos.length ; i++){ 
      if(yTiros[i] < 0){ // Se o tiro sair da tela, ele deixa de existir
        xTiros[i] = -1000; // Faz o tiro ir para uma posição que não aparece na tela
        yTiros[i] = -1000;
      }
    }
    if(vidas <= 0){
      tela = 3;
    }
    else if(nivel > 5){
      tela = 4;
    }

  }
  if(tela == 3){
    background(imgTela1);
    textFont("Times New Roman");
    textSize(50);
    text("DEU RUIM!!!", 80, 200);
    textSize(30);
    text("Para tentar novamente, \n   pressionea tecla F5.", 80, 300);

    
  }
  if(tela==4){
    background(imgTela1)
    textFont("Times New Roman");
    textSize(50);
    text("Parabéns, \nvocê ganhou!", 90, 200); 
    
  }
}
