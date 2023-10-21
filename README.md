# eth-zksync-multiple-addresses-script     
To automate sending ETH from one address to multiple addresses on the ZkSync network
from web3 import Web3
from web3.middleware import geth_poa_middleware 

# Connect to the ZkSync network
w3 = Web3(Web3.HTTPProvider('https://zksync-node-url'))

# Add middleware for handling ZkSync's Proof-of-Authority network
w3.middleware_onion.inject(geth_poa_middleware, layer=0)

# Load your private key or wallet JSON file for signing transactions
account = w3.eth.account.from_key('your-private-key')

# Specify the recipient addresses
to_addresses = ['address1', 'address2', 'address3']  # Replace with actual recipient addresses
amount = 0.1  # Amount of ETH to send

# Send transactions to each recipient address
for address in to_addresses:
    transaction = {
        'from': account.address,
        'to': address,
        'value': w3.toWei(amount, 'ether'),
        'gas': 21000,
        'gasPrice': w3.toWei('50', 'gwei')
    }

    signed_txn = account.sign_transaction(transaction)
    tx_hash = w3.eth.sendRawTransaction(signed_txn.rawTransaction)
    print(f'Transaction sent: {w3.toHex(tx_hash)}')
