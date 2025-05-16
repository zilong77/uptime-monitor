// Dependencies: React, Axios, Tailwind CSS
import React, { useEffect, useState } from 'react';
import axios from 'axios';

const RPC_URL = 'https://lumera-testnet-rpc.polkachu.com';
const API_URL = 'https://lumera-testnet-api.polkachu.com';

const base64ToHex = (base64) => {
  const binary = atob(base64);
  let hex = '';
  for (let i = 0; i < binary.length; i++) {
    hex += binary.charCodeAt(i).toString(16).padStart(2, '0');
  }
  return hex;
};

const sha256 = async (hex) => {
  const buffer = new Uint8Array(hex.match(/.{1,2}/g).map(byte => parseInt(byte, 16)));
  const hashBuffer = await crypto.subtle.digest('SHA-256', buffer);
  return Array.from(new Uint8Array(hashBuffer))
    .map(b => b.toString(16).padStart(2, '0'))
    .join('');
};

const pubkeyToAddress = async (base64) => {
  const hex = base64ToHex(base64);
  const hash = await sha256(hex);
  return hash.substring(0, 40).toUpperCase();
};

const UptimeDashboard = () => {
  const [validators, setValidators] = useState([]);
  const [latestHeight, setLatestHeight] = useState(0);
  const [uptimes, setUptimes] = useState({});

  useEffect(() => {
    const fetchLatestBlock = async () => {
      const res = await axios.get(`${RPC_URL}/status`);
      const height = parseInt(res.data.result.sync_info.latest_block_height);
      setLatestHeight(height);
    };
    fetchLatestBlock();
  }, []);

  useEffect(() => {
    const fetchValidatorsMetadata = async () => {
      const res = await axios.get(`${API_URL}/cosmos/staking/v1beta1/validators?status=BOND_STATUS_BONDED`);
      const meta = await Promise.all(res.data.validators.map(async (v) => {
        const pubkey = v.consensus_pubkey.key;
        const cons_address = await pubkeyToAddress(pubkey);
        return {
          moniker: v.description.moniker,
          cons_address,
          voting_power: parseInt(v.tokens)
        };
      }));
      setValidators(meta);
    };
    fetchValidatorsMetadata();
  }, []);

  useEffect(() => {
    const analyzeUptime = async () => {
      const newUptimes = {};
      for (const val of validators) {
        const address = val.cons_address;
        let count = 0;
        let statuses = [];

        for (let h = latestHeight; h > latestHeight - 30; h--) {
          const res = await axios.get(`${RPC_URL}/block?height=${h}`);
          const signatures = res.data.result.block.last_commit.signatures;
          const signed = signatures.some(sig => sig.validator_address === address && sig.block_id_flag === 2);
          statuses.push(signed ? 'Committed' : 'Missed');
          if (signed) count++;
        }

        newUptimes[address] = {
          moniker: val.moniker || address,
          statuses,
          percentage: ((count / 30) * 100).toFixed(2),
          voting_power: val.voting_power
        };
      }

      // Sort by voting power descending
      const sorted = Object.entries(newUptimes).sort(([, a], [, b]) => b.voting_power - a.voting_power);
      const sortedObject = Object.fromEntries(sorted);
      setUptimes(sortedObject);
    };

    if (validators.length && latestHeight) analyzeUptime();
  }, [validators, latestHeight]);

  return (
    <div className="min-h-screen bg-gray-100 p-6">
      <div className="max-w-6xl mx-auto">
        <h1 className="text-3xl font-bold text-gray-800 mb-8 text-center">Lumera Validator Uptime (Last 30 Blocks)</h1>
        <div className="space-y-6">
          {Object.entries(uptimes).map(([address, data]) => (
            <div key={address} className="bg-white rounded-xl shadow p-4">
              <div className="flex flex-col md:flex-row md:items-center md:justify-between mb-4">
                <h2 className="text-lg font-semibold text-gray-700">{data.moniker}</h2>
                <div className="text-sm text-gray-500">
                  <span className="mr-4">{data.percentage}% uptime</span>
                  <span>VP: {data.voting_power.toLocaleString()}</span>
                </div>
              </div>
              <div className="flex flex-row flex-wrap gap-1">
                {data.statuses.map((status, idx) => (
                  <div
                    key={idx}
                    className={`w-4 h-4 sm:w-5 sm:h-5 rounded-full border border-gray-200 ${
                      status === 'Committed' ? 'bg-green-500' : 'bg-red-500'
                    }`}
                    title={`Block ${latestHeight - 29 + idx}: ${status}`}
                  ></div>
                ))}
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default UptimeDashboard;
