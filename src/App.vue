<script setup>
import { ethers } from 'ethers'
import { ref } from 'vue';
import { ElMessage } from 'element-plus'

const account = ref('');

function getWeb3Provider() {
  if (!window.web3Provider) {
    if (!window.ethereum) return null;
    window.web3Provider = new ethers.BrowserProvider(window.ethereum)
  }
  return window.web3Provider;
}

async function connectWallet() {
  if (!getWeb3Provider()) {
    return ElMessage.error('未检测到 Web3 钱包提供商');
  }
  try {
    // 获取当前连接的账户地址:
    account.value = (await window.ethereum.request({
      method: 'eth_requestAccounts',
    }))[0];
    // 获取当前连接的链ID:
    await window.ethereum.request({
      method: 'eth_chainId'
    });
    ElMessage.success('钱包已连接');
    // eslint-disable-next-line no-unused-vars
  } catch (e) {
    ElMessage.error('无法获取钱包连接');
  }
}
</script>

<template>
  <div class="header">
    <router-link to="/">Storage Dapp</router-link>
    {{ account }}
    <el-button type="primary" plain @click="connectWallet">连接钱包</el-button>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px;
}
</style>
