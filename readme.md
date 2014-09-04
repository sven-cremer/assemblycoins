## Setup

    $ virtualenv venv
    $ source venv/bin/activate
    $ pip install -r requirements.txt --allow-all-external
    $ cp .env.sample .env
    $ # edit .env
    $ forego start

##API Calls

####API ROOT
- api.assets.assembly.com

####Colors

- #####Prompt API Server for New Coin Issuing Address
  - POST /v1/colors/prepare
        curl https://api.assets.assembly.com/v1/coins/prepare \
        -X POST \
        -d "coin_name"="mikoin" \
        -d "issued_amount=999" \
        -d "destination_address=TESTD" \
        -d "description=letsdoit" \
        -d "email=barisser@gmail.com"


    Response
    {"name": "mikoin", "issuing_private_key": "5KUABpsoZKMqpvm3yFe9Zg52QXhXY8Xw8pa4ntuK7SBdVt7CkrK", "minting_fee": "0.0004", "issuing_public_address": "1EmnqhfvjcAdA71gs2exugXkgHrJw9QcuA"}


- #####Check Holders of particular Coin Type
  - /v1/colors/"color_address"


    Response
    {"1PaCGhg1JtD4C6LrRLozjSDe5T2Uco1cAJ": 4, "19HjNMysWnjr5dpNhJxp7CZ4RejTkCsby6": 6}


- See metadata for all known Colors
  - GET /v1/colors/
      curl https://api.assets.assembly.com/v1/colors/3A5JTQS7ereJSfJCa6CVP8VNVSndyQD92s


    - Response
      - {"19aa71ZGwxTBDtazTKCHQvKoVJoEq71tEy": 1}


- Make New Coin Directly with Server Side Transaction Signing
  - POST /v1/colors/
      curl https://api.assets.assembly.com \
      -X POST \
      -d "public_address=1C1YLvSwh2imUsGnJ8qno1XgTKZMgcTcbp" \
      -d "initial_coins=137"  \
      -d "name=augusto"  \
      -d "recipient=173CJ9wxuZFbJyDbkJ89AfpAkqx5PatxMk" \
      -d "private_key=YOUR PRIVATE KEY HERE" \
      -d "description=Hey what a cool coin"


    - Response
      - "b9d3b5e409224eb1f1317932f7aaf97bad59510d5f7ecb4b83856d93f9a274f5"


####Addresses

- #####Check Address Balances
  - /v1/addresses/"public_address"


    curl https://api.assets.assembly.com/v1/addresses/1CEyiC8DXT6TS3d9iSDnXRBtwyPuVGRa9P

    Response
    {"1CEyiC8DXT6TS3d9iSDnXRBtwyPuVGRa9P": {"3N2bUx2XCWBfXzNd3YiDpFVAHQtSi1Yj5w": 10000}}


- #####Generate Public/Private Address Pair
  - /v1/addresses/


    curl https://api.assets.assembly.com/v1/addresses

    Response
    {'private_key': '5Hs2ztSw4T239kH2jDmm7nBTqycmsaVzQSxsE4MYrv3ogVhuM5J', 'public_address': '1JfMoC98NTxYiHwMDoA3TTiiW5cf7rXApY'}



####Transactions


- #####Transfer Colored Coins with Server Side signing
  - POST /v1/transactions/transfer


    curl https://api.assets.assembly.com/v1/colors/transfer \
      X POST \
      -d "from_public_address=" \
      -d "from_private_key= "  \
      -d "amount=" \
      -d "source_address=" \
      -d "to_public_address="

    Response
      {}

- #####Push Raw Transaction to Bitcoin Network
  - POST /v1/transactions


    curl https://api.assets.assembly.com/v1/transactions \
     -X POST \
     -d "transaction_hex="

    Response


  - #####Parsed Open Assets Transactions in Block
    - /v1/transactions/parsed/"Block Height"


    curl https://api.assets.assembly.com/v1/transactions/parsed/300712

    Response
    {"parsed_transactions": [{"parsed_colored_info": {"asset_quantities": [1000000], "version": "0100", "transferred": [], "metadata_length": 27, "asset_count": 1, "type": "OA", "metadata": "u=https://cpr.sm/bKAozZKLe1", "issued": [{"btc": 600, "txhash_index": "6e556f59cdd702f46260ba6b7b9af25ed6f11e757aa802d62584769aa96fd20b:0", "color_address": "3C22VLvs2GWqWCpBbu1L2xEfmtyJiVzt3w", "previous_inputs": "source:1D24qr4gDZ1h5D5hLXF3AbddFWiA4Vnm3d", "quantity": 1000000, "destination_address": "1D24qr4gDZ1h5D5hLXF3AbddFWiA4Vnm3d"}]}, "transaction_hash_with_index": "6e556f59cdd702f46260ba6b7b9af25ed6f11e757aa802d62584769aa96fd20b:1"}]}


  - #####Get Raw Transaction Information

    - /v1/transactions/raw/"TX HASH"


    curl https://api.assets.assembly/com/v1/transactions/raw/87e7d0c02b5c518e1b5d8668c6db423fbe0d5ad461e9e7f2086d52275d98d72d

    Response
    {"raw_transaction": {"vout": [{"value": 3.0, "n": 0, "scriptPubKey": {"hex": "76a9142f5befb369ed9cf1c04934387a7a55bffdf8ed8688ac", "type": "pubkeyhash", "asm": "OP_DUP OP_HASH160 2f5befb369ed9cf1c04934387a7a55bffdf8ed86 OP_EQUALVERIFY OP_CHECKSIG", "reqSigs": 1, "addresses": ["15KQts8aQ84uiskjEjHFe3ZPTRnXDDppAT"]}}, {"value": 7.69703, "n": 1, "scriptPubKey": {"hex": "76a914c985e97940bd881f6fcfcf4f0295476d66fb326488ac", "type": "pubkeyhash", "asm": "OP_DUP OP_HASH160 c985e97940bd881f6fcfcf4f0295476d66fb3264 OP_EQUALVERIFY OP_CHECKSIG", "reqSigs": 1, "addresses": ["1KNZEvnE6A6Y9ev1kpNfxbM5kj1YSe7roa"]}}], "time": 1409789874, "locktime": 0, "version": 1, "vin": [{"scriptSig": {"hex": "493046022100a7beee5f45a6e6c4bd4f3b91c1c3f7e95f91ea1b99cfc3ecc78d2eafb0b926d1022100848f01e8159df6ed0cf264d2b9cdd3ab4c75fe85d25b48853530e2cd6a3e2aaf0141044ab0b335f0cd9278991663560c578f1fc586a6b0a985873669dd2986c266d7812410c713bf8f45b458b8a7ba176b265f055cc34d2814c57c54bc2184737765d1", "asm": "3046022100a7beee5f45a6e6c4bd4f3b91c1c3f7e95f91ea1b99cfc3ecc78d2eafb0b926d1022100848f01e8159df6ed0cf264d2b9cdd3ab4c75fe85d25b48853530e2cd6a3e2aaf01 044ab0b335f0cd9278991663560c578f1fc586a6b0a985873669dd2986c266d7812410c713bf8f45b458b8a7ba176b265f055cc34d2814c57c54bc2184737765d1"}, "vout": 1, "txid": "7809e998ad62201031ce4af82a358d27d588de74dc6c4f647c617e419a8db2bc", "sequence": 4294967295}], "hex": "0100000001bcb28d9a417e617c644f6cdc74de88d5278d352af84ace31102062ad98e90978010000008c493046022100a7beee5f45a6e6c4bd4f3b91c1c3f7e95f91ea1b99cfc3ecc78d2eafb0b926d1022100848f01e8159df6ed0cf264d2b9cdd3ab4c75fe85d25b48853530e2cd6a3e2aaf0141044ab0b335f0cd9278991663560c578f1fc586a6b0a985873669dd2986c266d7812410c713bf8f45b458b8a7ba176b265f055cc34d2814c57c54bc2184737765d1ffffffff0200a3e111000000001976a9142f5befb369ed9cf1c04934387a7a55bffdf8ed8688ac58bce02d000000001976a914c985e97940bd881f6fcfcf4f0295476d66fb326488ac00000000", "blockhash": "00000000000000000e9481fa2399ddd32d8d29543e92fde915319a234a3758c4", "blocktime": 1409789874, "txid": "87e7d0c02b5c518e1b5d8668c6db423fbe0d5ad461e9e7f2086d52275d98d72d", "confirmations": 2}}

####Messages

  - #####Write Multipart Statement on the Blockchain
    POST '/v1/messages/'


    curl https://api.assets.assembly.com/v1/messages \ -X POST \
      -d "public_address=" \ -d "fee_each=0.00005" \ -d "private_key=" \ -d "message=Before the creation of Ea, Sauron was one of the countless lesser Ainur spirits created by Eru Iluvatar, known as the Maia. At this time he was known as Mairon the Admirable, and partook in the Ainulindale, or Music of the Ainur. "

    Response
    {}



  - #####Read stitched-together multi-part OP_RETURN statements issued by an address
    - GET /v1/messages/"public_address"


    curl http://api.assets.assembly.com/v1/messages/1N8onLuitcQR9V3HB9QSARyFV6hwxA99Sx

    Response
    {"statements": "{\"name\": \"pillars\", \"desc\": \"one small step\", \"total\": 52352}"}


####Meta
