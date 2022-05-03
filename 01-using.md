# Discovering BBGO

[I](https://smoothieware.org) discovered crypto a while ago, I was one of the users who lost Bitcoins in the 2013 hack of the MtGox exchange, and only a year after that I spent 300 BTC to buy a second-hand hard-drive.

Imagining where I would be if I had just hodl those coins in a cold wallet really stings.

Recently, looking back at those two bad decisions recently got me itching to get back into it, and around the beginning of the Corona Virus crisis, I started investing a bit of money into crypto again.

I also love programming, so I started looking into crypto bots. 

At first, I used Kucoin's [Spot Grid](https://kucoin.notion.site/kucoin/KuCoin-Trading-Bot-FAQ-a83bab1123094dea957851811a1a2fb7) trading bot.

![Kucoin grid trading](https://kucoin.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8fcaced3-1480-42e1-9d7e-22e23363466a%2FUntitled.gif?table=block&id=c8841c09-902f-4ff3-88a3-6f97e94204e9&spaceId=bd1a3261-3380-4fe3-94e2-0b8922ebdb2d&userId=&cache=v2)

It works pretty well, and doing everything inside a web interface is nice, but it didn't allow for much customization, and there were some features I wanted, and I had no way to add them.

So, I started to look into [Zenbot](https://github.com/DeviaVir/zenbot). It's Javascript (which I like), but it didn't have Grid trading (the strategy I like most).

So I spent two months implementing grid trading into it. As I went, the quality of the code was so bad, and the performance so slow, that I spent much more time fixing/improving things than actually doing anything helpful.

I was about to give up, when just to make sure I didn't miss anything, I searched once more for trading bot projects on Github, and found this new project named [BBGO](https://github.com/c9s/bbgo).

It was perfect. It natively did Grid trading (no need to add it myself), and it was BLAZZING fast (and the code was the cleanest of any such project I had seen).

The reason it's important that the project be fast, in my use case, is that I use genetic (and other) optimization algorythms to find the best strategies (using backtesting) to trade hundreds of coins at once. This results in running thousands of bots in parralel on a server. When I switched to BBGO, simulations that took weeks before with Zenbot, now took only hours with BBGO.

# Using BBGO

Getting started with BBGO is pretty straightforward.

You can follow [the instructions](https://github.com/c9s/bbgo) yourself, but here's what I do:

1, I run the automated setup script:

    bash <(curl -s https://raw.githubusercontent.com/c9s/bbgo/main/scripts/setup-grid.sh) binance

Running it will give an output like this: 

	╰─○ bash <(curl -s https://raw.githubusercontent.com/c9s/bbgo/main/scripts/setup-grid.sh) binance
	downloading...
	% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
									Dload  Upload   Total   Spent    Left  Speed
	0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
	100 17.9M  100 17.9M    0     0  14.6M      0  0:00:01  0:00:01 --:--:-- 35.1M
	downloaded successfully
	Enter your BINANCE API key: test
	Enter your BINANCE API secret: test
	generating your .env.local file...
	dotenv is configured successfully
	config file is generated successfully
	================================================================
	now you can edit your strategy config file bbgo.yaml to run bbgo
	you look like a pro user, you can edit the config by:

	vim bbgo.yaml

	To run bbgo just type: 

	./bbgo run

	To stop bbgo, just hit CTRL-C

This one is for the Binance exchange, and the grid strategy (this matters only in that it gives you "default" config files that reflect this). 

What's most important though, is that it gives you the bbgo binary also (the ``bbgo`` file), and a ``bbgo.yaml`` config file.

By default, the bbgo.yaml file looks like this:

	---
	exchangeStrategies:
	- on: binance
	grid:
		symbol: BTCUSDT
		quantity: 0.001
		gridNumber: 100
		profitSpread: 100.0
		upperPrice: 50_000.0
		lowerPrice: 10_000.0
		long: true

This configures the strategy (here we're using Grid to trade BTC versus USDT).

Another important file is the ENV file (it's hidden, it's named ``.env.local`` ), it looks like this by default:

    BINANCE_API_KEY=test
    BINANCE_API_SECRET=test

(Of course, here you input your Binance API keys, instead of ``test``).

Now that all of this is setup properly, you are ready to run BBGO yourself, simply do:

   ../bbgo run

And your adventure begins!

In other articles, we'll cover backtesting, the database, optimizations, but for now this should do as an introduction.

If you need any help with BBGO, you can create an issue in the [Github project](https://github.com/c9s/bbgo), or email me at wolf.arthur@gmail.com 







