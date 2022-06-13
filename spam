from telethon.sync import TelegramClient, errors
from time import sleep
from telethon.errors.rpcerrorlist import MessageTooLongError, PeerIdInvalidError
import dbm
def dbm_base():
	file = dbm.open( 'api.dbm' ,'c')
	try:
		file['api_id']
	except:
		file['api_id'] = input('Введите api_id:')
		file['api_hash'] = input('Введите api_hash:')
	file.close()
	return dbm.open( 'api.dbm' ,'r')
file = dbm_base()
api_id = int(file['api_id'].decode())
api_hash = file['api_hash'].decode()
client = TelegramClient('client', api_id, api_hash)
def dialog_sort(dialog):
    return dialog.unread_count
def spammer(client):
    def create_groups_list(groups=[]):
        for dialog in client.iter_dialogs():
            if dialog.is_group:
                if dialog.unread_count >= 5:
                    groups.append(dialog) 
        return groups
    with client:
        for m in client.iter_messages('me', 1):
            msg = m
        while True:
            groups = create_groups_list()
            groups.sort(key=dialog_sort, reverse=True)
            for g in groups[:90]:
                try:
                    client.forward_messages(g, msg, 'me')
                except errors.ForbiddenError as o:
                    client.delete_dialog(g)
                    if g.entity.username != None:
                        print(f'Error: {o.message} Аккаунт покинул @{g.entity.username}')
                    else:
                        print(f'Error: {o.message} Аккаунт покинул {g.name}')
                except errors.FloodError as e:
                	print(f'Error: {e.message} Требуется ожидание {e.seconds} секунд')
                	sleep(e.seconds)
                except PeerIdInvalidError:
                    client.delete_dialog(g)
                except MessageTooLongError:
                    print(f'Message was too long ==> {g.name}')                                     
                except errors.BadRequestError as i:
                	print(f'Error: {i.message}')
                except errors.RPCError as a:
                	print(f'Error: {a.message}')
            sleep(1200)
            groups.clear()
if __name__ == '__main__':
	spammer(client)
