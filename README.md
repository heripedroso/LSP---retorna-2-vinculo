# LSP---retorna-2-vinculo
Função que retorna um 2º vínculo caso o colaborador possua um outro cadastro ativo no sistema.

## Função que preenche a 2ª posição da estrutura objColaborador[].
* Nesta função utiliza-se o CPF para encontrar outro cadastro ativo, no entanto pode-se utilizar também o PIS.
```
Definir Funcao retorna2Vinculo();

Funcao retorna2Vinculo();
{
     @ ----------------Carrega valores ------------------------ @
     nNumCpf = objColaborador[1].nNumCpf;
     nNumEmp = objColaborador[1].nNumEmp; 
     nTipCol = objColaborador[1].nTipCol;
     nNumCad = objColaborador[1].nNumCad;
          
     Definir Data dDataZero;
     MontaData (31, 12, 1900, dDataZero);     
     Definir Data dInicioMes; dInicioMes = objParamentrosEntrada[1].dDataInicioDoMes;
     Definir Data dFimMes; dFimMes = objParamentrosEntrada[1].dDataFimDoMes;
     
     Definir Cursor c2Vinculo;  
     @ --- Procura por outro vínculo em um determinado período ---  @
     c2Vinculo.SQL "Select R034FUN.NumEmp, R034FUN.TipCol, R034FUN.NomFun, R034FUN.USU_NUMSEAD ,R034FUN.USU_VINSEAD, R034FUN.numcad, R034FUN.NumCra , R024CAR.TITCAR, R024CAR.CodCar From R034FUN, R024CAR \
               WHERE R034FUN.CODCAR = R024CAR.CODCAR and R034FUN.NUMCPF = :nNumCPF and R034FUN.NUMCAD <> :nNumCad and R034FUN.TIPCOL = :nTipCol \
               AND ( R034FUN.DatAfa <> :dDataZero AND EXISTS(SELECT R010SIT.CODSIT FROM R010SIT WHERE R010SIT.CODSIT = R034FUN.SITAFA AND R010SIT.TIPSIT IN (7)) AND :dInicioMes <= R034FUN.DatAfa AND :dFimMes >= R034FUN.datAdm \
                     OR \ 
                     R034FUN.DatAfa <> :dDataZero AND NOT EXISTS(SELECT R010SIT.CODSIT FROM R010SIT WHERE R010SIT.CODSIT = R034FUN.SITAFA AND R010SIT.TIPSIT IN (7)) AND R034FUN.datAdm <= :dFimMes \   
                     OR \ 
                     R034FUN.DatAfa =  :dDataZero AND R034FUN.datAdm <= :dFimMes )";                

     @ --- Carrega valores em objColaborador[2] ---  @          
     c2Vinculo.AbrirCursor();
     Se (c2Vinculo.Achou) {  
           objColaborador[2].nNumCpf = nNumCpf;
           objColaborador[2].nNumEmp = c2Vinculo.NumEmp;
           objColaborador[2].nTipCol = c2Vinculo.TipCol;
           objColaborador[2].nNumCad = c2Vinculo.NumCad;
           objColaborador[2].nNumCra = c2Vinculo.NumCra;
           objColaborador[2].aNome = c2Vinculo.NomFun;
           objColaborador[2].nNumSead = c2Vinculo.USU_NUMSEAD;
           objColaborador[2].nVinculoSead = c2Vinculo.USU_VINSEAD;              
           objColaborador[2].aCargo = c2Vinculo.TitCar;
           objColaborador[2].aCodCargo = c2Vinculo.CodCar;
     }
     c2Vinculo.FecharCursor();
};
```

## Recomendações
Para usar a função na regra, deve-se declará-la no início da mesma.
É interessante que se reserve, por exemplo, a regra 001 apenas para implementar funções.

Em implementações de relatório, tenho o hábito de declarar as funções em Funções Globais no Modelo Gerador.

![image](https://github.com/heripedroso/LSP---converte-minutos-em-HH-MI/assets/22459829/fa6ef8f7-399d-4923-9c2e-a814f502bddc)
