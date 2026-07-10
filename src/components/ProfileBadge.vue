/template/
&lt;div class="profile-badge"&gt;
  &lt;img :src="/images/profile/profile.jpg" alt="Kevin Octavius" /&gt;
  &lt;span&gt;Kevin Octavius&quot;/&gt;&lt;;</span>&gt;
&lt;/div&gt;

&lt;style scoped&gt;
.profile-badge {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 16px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
}

img {
  width: 48px;
  height: 48px;
  object-fit: cover;
  border-radius: 50%;
  border: 2px solid rgba(124, 58, 237, 0.4);
}

span {
  font-weight: 600;
  color: #fff;
  letter-spacing: -0.01em;
}
&lt;/style&gt;&quot;