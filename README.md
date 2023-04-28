from aiogram import Bot, Dispatcher, executor, types
from inCommand import ikb, kb, InlineKeyboardMarkup, InlineKeyboardButton
from beton import TOKEN_API

bot = Bot(TOKEN_API)
dp = Dispatcher(bot)

HELP ="""
<em>/help</em> - <b>список команд</b>
<em>/photo</em> - <b>отправляет фотографию</b>
<em>/links</em> - <b>нужные ссылк в интернет</b>
<em>/vote</em> - <b>даёт фото на оценку</b>"""

async def on_startup(_):
    print('Программа запущена, Сэр!')

@dp.message_handler(commands=['start'])
async def start_command(message: types.Message):
    await message.answer(text='Здравствуйте Сэр!',
                         reply_markup=kb)
    await message.delete()

@dp.message_handler(commands=['links'])
async def command_links(message: types.Message):
    await message.answer(text='Выбирете опцию ...',
                         reply_markup=ikb)
    await message.delete()

@dp.message_handler(commands=['help'])
async def send_help(message: types.Message):
    await message.answer(text=HELP,
                         reply_markup=kb,
                         parse_mode="HTML")
    await message.delete()

@dp.message_handler(commands=['photo'])
async def send_command(message: types.Message):
    await bot.send_photo(chat_id=message.from_user.id,
                         photo='https://zastavok.net/leto/63383-holmy_zelen_ris.html',
                         reply_markup=kb)
    await message.delete()

@dp.message_handler(commands=['vote'])
async def vote_command(message: types.Message):
    ikb2 =InlineKeyboardMarkup(row_width=2)
    ib3 = InlineKeyboardButton(text='❤️',
                               callback_data='like')
    ib4 = InlineKeyboardButton(text='👎',
                               callback_data='dislike')
    ikb2.add(ib3, ib4)

    await bot.send_photo(chat_id=message.from_user.id,
                         photo='https://i.7fon.org/1000/f64433345.jpg',
                         caption='Нравиться картинка?',
                         reply_markup=ikb2)

@dp.callback_query_handler()
async def send_query(callback: types.CallbackQuery):
    if callback.data == 'like':
        await callback.answer(text='Ух ты, тебе нравился мой выбор')
    await callback.answer(text='Не чего страшного, найдём другую')


if __name__ == '__main__':
    executor.start_polling(dispatcher=dp,
                           on_startup=on_startup,
                           skip_updates=True)

__________________________________________________________________________________________________
#папка inCommand
__________________________________________________________________________________________________

from aiogram.types import ReplyKeyboardMarkup, KeyboardButton, InlineKeyboardButton, InlineKeyboardMarkup

ikb = InlineKeyboardMarkup(row_width=2)
ib1 = InlineKeyboardButton(text='Yandex',
                           url='https://yandex.ru/search/?text=%D1%8F%D0%BD%D0%B4%D0%B5%D0%BA%D1%81&clid=2411726&lr=10991')
ib2 = InlineKeyboardButton(text='YouTube',
                           url='https://www.youtube.com/@KINOKOS')

ikb.add(ib1, ib2)
kb = ReplyKeyboardMarkup(resize_keyboard=True,
                         one_time_keyboard=True)
v1 = KeyboardButton(text='/links')
v2 = KeyboardButton(text='/help')
v3 = KeyboardButton(text='/photo')
v4 = KeyboardButton(text='/vote')
kb.add(v1).insert(v2).add(v3).insert(v4)
