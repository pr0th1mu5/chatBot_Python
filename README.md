# chatBot_Python
### Um jeito interessante de otimizar as conversas entre docente e estudantes em tempos de pandemia.

## chatBot kinBot


Galera, se está disponível aí o código para todos possam modificá-lo e reutilizá-lo. 
A ideia foi inicial para eu ter como dar respostas rápidas a todos os meus estudantes, mas logo vi que outros professores também vão precisar. 

Vamos unir forças e prestar um trabalho voluntário solidário para ajudar a todos os guerreiros professores que precisam dessa ferramenta e fazer o nosso país tomar outro rumo. A solução sempre foi a educação! Conto com a ajuda de todos! Abraço.

------------------------------------------------------------------------------------------

import requests
import time
import json
import os


class TelegramBot:
    def __init__(self):
        token = '1795988840:AAEmbKuDrOTcloaPi_ErV4snorvEELCulpA'
        self.url_base = f'https://api.telegram.org/bot{token}/'


#Inicia o chat pegando os dados que interessam das mensagens e chama fução para criar resposta se ela for diferente de "None", que é quando alguém digita qualquer coisa que não seja "professor"

    def Iniciar(self):
        update_id = None
        while True:
            atualizacao = self.obter_novas_mensagens(update_id)
            dados = atualizacao['result']
            if dados:
                for dado in dados:
                    update_id = dado['update_id']
                    mensagem = str(dado["message"]["text"])
                    chat_id = dado["message"]["from"]["id"]                    
                    resposta = self.criar_resposta(mensagem)
                    if resposta != None:
                      self.responder(resposta, chat_id)
                  

#Obter mensagens escritas no chat e verifica sempre a seguinte enviada a cada 100 milissegundos. 

    def obter_novas_mensagens(self, update_id):
        link_requisicao = f'{self.url_base}getUpdates?timeout=100'
        if update_id:
            link_requisicao = f'{link_requisicao}&offset={update_id + 1}'
        resultado = requests.get(link_requisicao)
        return json.loads(resultado.content)

#Criar uma resposta. Aqui o bot só responde se a palavra chamada lá no chat for "professor". Não há nenhum outro tipo de interação e nem como os estudantes saber que ele vai responder sem que o administrador do grupo avise antes. Isso será informado no cabeçalho de detalhamento do grupo.

    def criar_resposta(self, mensagem):       
        if mensagem.lower() == 'professor':
          return f'''Fala óh ser vivente que interage por esse latifúndio digital. Digite "professor" sempre que precisar de alguma ajuda =){os.linesep}{os.linesep}Escolha um número correspondente a uma das opções para ver se posso te ajudar:{os.linesep}{os.linesep}1 - Dúvida sobre aulas{os.linesep}2 - Dúvida sobre atividades{os.linesep}3 - Material de Apoio{os.linesep}4 - Outros assuntos'''
        if mensagem == '1':
            return f'''Nossas aulas vão ser ministradas às Terças e Quintas. {os.linesep}Na terça será a partir das 14 horas via googleMeet. {os.linesep}Não falte! As quintas será as 16h também via meet. {os.linesep}{os.linesep}Espero ter ajudado! Outra dúvida só digitar "professor" =)'''            
        elif mensagem == '2':
            return f'''Nossas atividades vão ser mistas. Como assim? {os.linesep}Respondendo: algumas vão ser no momento da aula, algumas via sigaa e outras através de formulários via ferramentas do Google. {os.linesep}Assim teremos várias atividades para avançarmos em conhecimento.{os.linesep} {os.linesep}Se tirei a dúvida, abraço! Se não, digita aí de novo "professor" de novo.
            '''
        elif mensagem == '3':
            return f'''Os materiais e apoio vão ser passados aos poucos assim que formos avançando em teoria e prática. {os.linesep}Mas se preoucupe não que postarei aqui também.{os.linesep} {os.linesep}Se tirei a dúvida, abraço! Se não, digita aí de novo "professor" de novo.
            '''
        elif mensagem == '4':
            return f'''Eu aconselho a mandar perguntas que não são sobre o horário e as atividades no privado. {os.linesep} Se for algo sério e de urgência, direto pro e-mail do meu criador: cleciosousa@ufpi.edu.br por que ele sempre olha os e-mails.{os.linesep}{os.linesep}Se era isso, é issAew... senão, digita "professor" de novo pra ver o menu novamente. ;)'''                            
    #Essa função vai Responder após criar a resposta na função anterior.
    def responder(self, resposta, chat_id):      
        link_requisicao = f'{self.url_base}sendMessage?chat_id={chat_id}&text={resposta}'
        
        requests.get(link_requisicao)
        '''A partir desse ponto, volte as chamadas logo a frente para a primeira coluna do código python. Não conseguir fazer isso no git ;)'''
        bot = TelegramBot() 
        bot.Iniciar()
-------------------------------------------------------------------------------
#### Fim do código.

    
    
    
