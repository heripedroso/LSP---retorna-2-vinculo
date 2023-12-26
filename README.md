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
     /* --- Procura por outro vínculo em um determinado período ---  */ 
     c2Vinculo.SQL "Select R034FUN.NumEmp, R034FUN.TipCol, R034FUN.NomFun, R034FUN.USU_NUMSEAD ,R034FUN.USU_VINSEAD, R034FUN.numcad , R024CAR.TITCAR From R034FUN, R024CAR \
               WHERE R034FUN.CODCAR = R024CAR.CODCAR and R034FUN.NUMCPF = :nNumCPF and R034FUN.NUMCAD <> :nNumCad and R034FUN.TIPCOL = :nTipCol \
               AND ( R034FUN.DatAfa <> :dDataZero AND EXISTS(SELECT R010SIT.CODSIT FROM R010SIT WHERE R010SIT.CODSIT = R034FUN.SITAFA AND R010SIT.TIPSIT IN (7)) AND :dInicioMes <= R034FUN.DatAfa AND :dFimMes >= R034FUN.datAdm \
                     OR \ 
                     R034FUN.DatAfa <> :dDataZero AND NOT EXISTS(SELECT R010SIT.CODSIT FROM R010SIT WHERE R010SIT.CODSIT = R034FUN.SITAFA AND R010SIT.TIPSIT IN (7)) AND R034FUN.datAdm <= :dFimMes \   
                     OR \ 
                     R034FUN.DatAfa =  :dDataZero AND R034FUN.datAdm <= :dFimMes )";                
               
     c2Vinculo.AbrirCursor();
     Se (c2Vinculo.Achou) {  
         /*
           Preenche valores em objColaborador[2].
         */      
     }
     c2Vinculo.FecharCursor();


};
```
