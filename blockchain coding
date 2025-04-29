import streamlit as st
import hashlib
import json
import time

# ----------------- Block Class -----------------
class Block:
    def __init__(self, index, data, previous_hash):
        self.index = index
        self.timestamp = time.ctime()
        self.data = data
        self.previous_hash = previous_hash
        self.hash = self.compute_hash()

    def compute_hash(self):
        block_string = json.dumps({
            "index": self.index,
            "timestamp": self.timestamp,
            "data": self.data,
            "previous_hash": self.previous_hash
        }, sort_keys=True)
        return hashlib.sha256(block_string.encode()).hexdigest()

# ----------------- Blockchain Class -----------------
class Blockchain:
    def __init__(self):
        self.chain = []
        self.create_genesis_block()

    def create_genesis_block(self):
        genesis_data = {
            "parcel_id": "0000",
            "status": "Parcel Created",
            "location": "N/A"
        }
        genesis_block = Block(0, genesis_data, "0")
        self.chain.append(genesis_block)

    def add_block(self, data):
        previous_block = self.chain[-1]
        new_block = Block(len(self.chain), data, previous_block.hash)
        self.chain.append(new_block)

    def is_chain_valid(self):
        for i in range(1, len(self.chain)):
            curr = self.chain[i]
            prev = self.chain[i - 1]
            if curr.hash != curr.compute_hash():
                return False
            if curr.previous_hash != prev.hash:
                return False
        return True

# ----------------- Streamlit App -----------------
st.set_page_config(page_title="Parcel Tracker Blockchain", layout="centered")

# Initialize blockchain in session state
if "blockchain" not in st.session_state:
    st.session_state.blockchain = Blockchain()

st.title("📦 Parcel Tracking on Blockchain")

# Add new block form
st.subheader("Add Parcel Status")
with st.form("add_block_form"):
    parcel_id = st.text_input("Parcel ID")
    status = st.text_input("Status")
    location = st.text_input("Location")
    submitted = st.form_submit_button("Add Block")

    if submitted and parcel_id and status and location:
        data = {
            "parcel_id": parcel_id,
            "status": status,
            "location": location
        }
        st.session_state.blockchain.add_block(data)
        st.success("Block added to the blockchain!")

# Display Blockchain
st.subheader("📄 Blockchain Ledger")
for block in st.session_state.blockchain.chain:
    with st.expander(f"Block {block.index} | {block.data['status']}"):
        st.write("**Parcel ID:**", block.data["parcel_id"])
        st.write("**Status:**", block.data["status"])
        st.write("**Location:**", block.data["location"])
        st.write("**Timestamp:**", block.timestamp)
        st.code(block.hash, language="text")
        st.caption("Previous Hash: " + block.previous_hash)

# Validate Blockchain
st.subheader("✅ Blockchain Validity Check")
if st.session_state.blockchain.is_chain_valid():
    st.success("Blockchain is valid ✅")
else:
    st.error("Blockchain has been tampered with ❌")
