# NFTPokemon

## Crie o seu NFT de Pokémon com Blockchain

Foi desenvolvido um contrato inteligente para criar um NFT com sistema de troca e batalhas entre pokémons. Foram utilizadas as ferramentas:

* Ganache: Ambiente de Desenvolvimento Ethereum
* MetaMask: Carteira Crypto para navegadores Web
* Remix: Ethereum IDE
* IPFS: Sistema de arquivos distribuído

Com o Ganache, MetaMask e IPFS instalados, foi digitado o seguinte código no Remix:

```
// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.26;


import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract PokeDIO is ERC721{

    struct Pokemon{
        string name;
        uint level;
        string img;
    }

    Pokemon[] public pokemons;
    address public gameOwner;

    constructor () ERC721 ("PokeDIO", "PKD"){

        gameOwner = msg.sender;

    } 

    modifier onlyOwnerOf(uint _monsterId) {

        require(ownerOf(_monsterId) == msg.sender,"Apenas o dono pode batalhar com este Pokemon");
        _;

    }

    function battle(uint _attackingPokemon, uint _defendingPokemon) public onlyOwnerOf(_attackingPokemon){
        Pokemon storage attacker = pokemons[_attackingPokemon];
        Pokemon storage defender = pokemons[_defendingPokemon];

         if (attacker.level >= defender.level) {
            attacker.level += 2;
            defender.level += 1;
        }else{
            attacker.level += 1;
            defender.level += 2;
        }
    }

    function createNewPokemon(string memory _name, address _to, string memory _img) public {
        require(msg.sender == gameOwner, "Apenas o dono do jogo pode criar novos Pokemons");
        uint id = pokemons.length;
        pokemons.push(Pokemon(_name, 1,_img));
        _safeMint(_to, id);
    }

}
```



No menu lateral, em "Solidity Compiler", clicar em "Compile PokeDIO.sol". Depois no menu "Deploy & run transactions"no campo "Environment", selecionar "Injected Provider - Metamask" e conectar a sua Carteira MetaMask. No campo "Contract" Selecionar "PokeDIO - contracts/DIOPokemonNFT/PokeDIO.sol". Clicar em "Deploy".

As imagens devem ser carregadas no IPFS e o link de acesso a elas via navegador deve estar disponível para usar na execução do contrato. Temos várias opções em como interagir com o contrato, entre elas transferir e batalhar.
