import React, { useState, useEffect, useMemo } from 'react';
import { RefreshCcw } from 'lucide-react'; // Importando um ícone de retorno mais sofisticado

// Componente para o Modal de Duplicidade
const DuplicateModal = ({ isOpen, onClose, shipment }) => {
  if (!isOpen) return null;

  return (
    <div className="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50 p-4">
      <div className="bg-gray-800 text-white p-8 rounded-lg shadow-xl max-w-sm w-full text-center border border-gray-700">
        <h3 className="text-2xl font-bold text-red-400 mb-4">Entrada Duplicada!</h3>
        <p className="text-gray-300 mb-6">
          A remessa "<span className="font-semibold text-white">{shipment}</span>" já foi bipada anteriormente.
        </p>
        <button
          onClick={onClose}
          className="bg-blue-600 text-white px-6 py-3 rounded-md hover:bg-blue-500 transition duration-300 ease-in-out shadow-lg"
        >
          Entendi
        </button>
      </div>
    </div>
  );
};

// Componente para o Modal de Geração de Texto
const TextModal = ({ isOpen, onClose, title, content }) => {
  if (!isOpen) return null;

  const handleCopy = () => {
    // Implementa a cópia do texto para a área de transferência
    const el = document.createElement('textarea');
    el.value = content;
    document.body.appendChild(el);
    el.select();
    document.execCommand('copy');
    document.body.removeChild(el);
    alert('Texto copiado para a área de transferência!');
  };

  return (
    <div className="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50 p-4">
      <div className="bg-gray-800 text-white p-8 rounded-lg shadow-xl max-w-lg w-full border border-gray-700">
        <h3 className="text-2xl font-bold text-gray-100 mb-4">{title}</h3>
        <div className="bg-gray-700 p-4 rounded-md mb-4 overflow-y-auto max-h-64 whitespace-pre-wrap text-gray-200">
          <p>{content}</p>
        </div>
        <div className="flex justify-end gap-4">
          <button
            onClick={handleCopy}
            className="bg-green-600 text-white px-6 py-3 rounded-md hover:bg-green-500 transition duration-300 ease-in-out shadow-lg"
          >
            Copiar Texto
          </button>
          <button
            onClick={onClose}
            className="bg-red-500 text-white px-6 py-3 rounded-md hover:bg-red-400 transition duration-300 ease-in-out shadow-lg"
          >
            Fechar
          </button>
        </div>
      </div>
    </div>
  );
};

// Define o componente principal da aplicação
const App = () => {
  // Estados para as informações de entrada
  const [carrier, setCarrier] = useState('');
  const [shipment, setShipment] = useState('');
  // Estado para armazenar a lista de entradas bipadas
  const [materials, setMaterials] = useState([]);
  // Estado para controlar a página atual ('bipagem' ou 'relatorio')
  const [currentPage, setCurrentPage] = useState('bipagem');
  // Estados para o modal de duplicidade
  const [showDuplicateModal, setShowDuplicateModal] = useState(false);
  const [duplicateShipment, setDuplicateShipment] = useState('');
  // Estados para o modal de geração de texto
  const [showTextModal, setShowTextModal] = useState(false);
  const [textModalTitle, setTextModalTitle] = useState('');
  const [textModalContent, setTextModalContent] = useState('');

  // Estados para os filtros do relatório
  const [filterCarrier, setFilterCarrier] = useState('');
  const [filterShipment, setFilterShipment] = useState('');
  const [filterDate, setFilterDate] = useState('');

  // Dados de exemplo para Transportadoras, simulando a origem de uma planilha de Configs
  const carriers = [
    "Selecione a Transportadora", // Opção padrão para bipagem
    "MANDOU BEM",
    "DISSUDES",
    "LOGAN",
    "GFL",
    "AZUL",
    "FAST"
  ];

  // Opções de transportadoras para o filtro, incluindo "Todas"
  const filterCarriersOptions = ["Todas", ...carriers.slice(1)]; // Remove "Selecione a Transportadora" e adiciona "Todas"

  // Opções de status para a nova coluna
  const statusOptions = [
    'RECEBIDO',
    'RECUSADO(AVARIA)',
    'FÍSICOS NÃO ENVIADOS',
    'RECUSADO(ETIQUETA NÃO BIPA)',
    'RECUSADO (DUAS ETIQUETAS)',
  ];

  // useEffect para adicionar a entrada automaticamente quando shipment muda (simulando bipagem)
  useEffect(() => {
    // Verifica se a remessa não está vazia e se a transportadora foi selecionada
    if (
      shipment.trim() !== '' &&
      carrier.trim() !== '' && carrier !== "Selecione a Transportadora"
    ) {
      // Verifica se a remessa já existe na lista
      const isDuplicate = materials.some(
        (material) => material.shipment === shipment.trim()
      );

      if (isDuplicate) {
        setDuplicateShipment(shipment.trim());
        setShowDuplicateModal(true);
        setShipment(''); // Limpa o campo da remessa mesmo se for duplicado
      } else {
        // Adiciona a nova entrada com todas as informações e um timestamp
        setMaterials(prevMaterials => [
          ...prevMaterials,
          {
            carrier: carrier.trim(),
            shipment: shipment.trim(),
            timestamp: new Date().toLocaleString(), // Data e hora atual
            status: 'RECEBIDO' // Define um status inicial
          }
        ]);
        setShipment(''); // Limpa o campo de input da remessa
        // Não limpa o campo da transportadora para facilitar múltiplas entradas com os mesmos dados
      }
    }
  }, [shipment, carrier, materials]); // Dependências atualizadas para incluir 'materials'

  // Função para lidar com a mudança no campo de seleção da transportadora (bipagem)
  const handleCarrierChange = (e) => {
    setCarrier(e.target.value);
  };

  // Função para lidar com a mudança no campo de input da remessa (bipagem)
  const handleShipmentChange = (e) => {
    setShipment(e.target.value);
  };

  // Função para exportar os dados para um arquivo CSV
  const handleExportCsv = () => {
    if (materials.length === 0) {
      alert("Não há dados para exportar.");
      return;
    }

    // Define o cabeçalho do CSV
    const headers = ["Transportadora", "Remessa", "Data e Hora", "Status"];
    // Mapeia os dados para o formato CSV
    const csvRows = materials.map(material =>
      ${material.carrier},${material.shipment},"${material.timestamp}",${material.status}
    );

    // Combina o cabeçalho e as linhas de dados
    const csvString = [headers.join(","), ...csvRows].join("\n");

    // Cria um Blob (Binary Large Object) a partir da string CSV
    const blob = new Blob([csvString], { type: 'text/csv;charset=utf-8;' });
    // Cria um URL para o Blob
    const url = URL.createObjectURL(blob);

    // Cria um link temporário para download
    const link = document.createElement('a');
    link.setAttribute('href', url);
    link.setAttribute('download', 'entradas_materiais.csv'); // Nome do arquivo

    // Adiciona o link ao corpo do documento, clica nele e remove
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    URL.revokeObjectURL(url); // Libera o URL do Blob
  };

  // Função para fechar o modal de duplicidade
  const handleCloseDuplicateModal = () => {
    setShowDuplicateModal(false);
    setDuplicateShipment('');
  };
  
  // Função para atualizar o status de uma entrada específica
  const handleStatusChange = (index, newStatus) => {
    const updatedMaterials = [...materials];
    updatedMaterials[index].status = newStatus;
    setMaterials(updatedMaterials);
  };

  // Lógica de filtragem dos materiais
  const filteredMaterials = useMemo(() => {
    return materials.filter(material => {
      // Filtro por Transportadora
      const matchesCarrier = filterCarrier === 'Todas' || filterCarrier === '' || material.carrier === filterCarrier;

      // Filtro por Remessa (busca parcial, case-insensitive)
      const matchesShipment = material.shipment.toLowerCase().includes(filterShipment.toLowerCase());

      // Filtro por Data (compara apenas a parte da data)
      const matchesDate = !filterDate || material.timestamp.split(',')[0].trim() === filterDate;

      return matchesCarrier && matchesShipment && matchesDate;
    });
  }, [materials, filterCarrier, filterShipment, filterDate]);

  // Função para gerar a sugestão de ação com a API Gemini
  const handleGenerateAction = async (material) => {
    const prompt = Como um assistente de logística, gere uma sugestão de ação para a seguinte remessa que foi recusada por avaria: Transportadora: ${material.carrier}, Remessa: ${material.shipment}, Data: ${material.timestamp}. A resposta deve ser um parágrafo conciso, focado no próximo passo a ser tomado.;
    
    setTextModalTitle('✨ Sugestão de Ação');
    setTextModalContent('Gerando sugestão...');
    setShowTextModal(true);

    try {
      let chatHistory = [];
      chatHistory.push({ role: "user", parts: [{ text: prompt }] });
      const payload = { contents: chatHistory };
      const apiKey = ""
      const apiUrl = https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey};
      const response = await fetch(apiUrl, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
      });
      const result = await response.json();
      if (result.candidates && result.candidates.length > 0 &&
          result.candidates[0].content && result.candidates[0].content.parts &&
          result.candidates[0].content.parts.length > 0) {
        setTextModalContent(result.candidates[0].content.parts[0].text);
      } else {
        setTextModalContent('Não foi possível gerar a sugestão. Tente novamente mais tarde.');
      }
    } catch (error) {
      console.error('Erro ao chamar a API Gemini:', error);
      setTextModalContent('Ocorreu um erro ao gerar a sugestão. Verifique sua conexão.');
    }
  };

  // Função para gerar o rascunho de e-mail com a API Gemini
  const handleGenerateEmail = async (material) => {
    const prompt = Como um assistente de logística, redija um rascunho de e-mail profissional e direto para notificar a transportadora. Inclua a transportadora, o número da remessa e o status na sua redação. A remessa é da transportadora: ${material.carrier}, com o número de remessa: ${material.shipment}, e o status é: ${material.status}. O assunto do e-mail deve ser: "Notificação de Status de Remessa - ${material.shipment}".;
    
    setTextModalTitle('✨ Rascunho de E-mail');
    setTextModalContent('Gerando rascunho de e-mail...');
    setShowTextModal(true);

    try {
      let chatHistory = [];
      chatHistory.push({ role: "user", parts: [{ text: prompt }] });
      const payload = { contents: chatHistory };
      const apiKey = ""
      const apiUrl = https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey};
      const response = await fetch(apiUrl, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
      });
      const result = await response.json();
      if (result.candidates && result.candidates.length > 0 &&
          result.candidates[0].content && result.candidates[0].content.parts &&
          result.candidates[0].content.parts.length > 0) {
        setTextModalContent(result.candidates[0].content.parts[0].text);
      } else {
        setTextModalContent('Não foi possível gerar o e-mail. Tente novamente mais tarde.');
      }
    } catch (error) {
      console.error('Erro ao chamar a API Gemini:', error);
      setTextModalContent('Ocorreu um erro ao gerar o e-mail. Verifique sua conexão.');
    }
  };

  // Renderiza a interface do usuário
  return (
    <div className="min-h-screen bg-gray-900 text-gray-100 flex items-center justify-center p-4">
      <div className="bg-gray-800 p-8 rounded-lg shadow-xl w-full max-w-3xl border border-gray-700">
        <h1 className="text-3xl font-bold text-center text-white mb-6 flex items-center justify-center gap-3">
          <RefreshCcw className="text-blue-500" size={32} /> {/* Ícone de retorno mais sofisticado */}
          ENTRADA DE REVERSA DAFITI
        </h1>

        {/* Navegação por abas */}
        <div className="flex justify-center mb-6 border-b border-gray-700">
          <button
            onClick={() => setCurrentPage('bipagem')}
            className={`py-2 px-4 text-lg font-medium ${
              currentPage === 'bipagem'
                ? 'border-b-2 border-blue-500 text-blue-500'
                : 'text-gray-400 hover:text-gray-200'
            } focus:outline-none`}
          >
            Bipagem
          </button>
          <button
            onClick={() => setCurrentPage('relatorio')}
            className={`py-2 px-4 text-lg font-medium ${
              currentPage === 'relatorio'
                ? 'border-b-2 border-blue-500 text-blue-500'
                : 'text-gray-400 hover:text-gray-200'
            } focus:outline-none`}
          >
            Relatório
          </button>
        </div>

        {/* Botão de Ação Global (Exportar) */}
        <div className="flex justify-center mb-6">
          <button
            onClick={handleExportCsv}
            className="flex-grow bg-green-600 text-white px-5 py-3 rounded-md hover:bg-green-500 transition duration-300 ease-in-out shadow-lg"
          >
            Exportar para CSV
          </button>
        </div>

        {/* Conteúdo condicional da aba */}
        {currentPage === 'bipagem' && (
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            {/* Campo para Transportadora (Dropdown) */}
            <div>
              <label htmlFor="carrier" className="block text-sm font-medium text-gray-300 mb-1">Transportadora</label>
              <select
                id="carrier"
                value={carrier}
                onChange={handleCarrierChange}
                className="w-full p-3 border border-gray-600 bg-gray-700 text-gray-200 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
              >
                {carriers.map((option, index) => (
                  <option key={index} value={option === "Selecione a Transportadora" ? "" : option} disabled={option === "Selecione a Transportadora"}>
                    {option}
                  </option>
                ))}
              </select>
            </div>

            {/* Campo para Remessa (Input de Texto) */}
            <div className="md:col-span-1">
              <label htmlFor="shipment" className="block text-sm font-medium text-gray-300 mb-1">Remessa (bipagem)</label>
              <input
                type="text"
                id="shipment"
                value={shipment}
                onChange={handleShipmentChange}
                placeholder="Bipe o número da remessa aqui..."
                className="w-full p-3 border border-gray-600 bg-gray-700 text-gray-200 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                autoFocus
              />
            </div>
            {materials.length === 0 && (
              <p className="text-center text-gray-400 mt-8 md:col-span-2">
                Nenhuma entrada registrada ainda. Selecione a transportadora e bipe a remessa!
              </p>
            )}
          </div>
        )}

        {currentPage === 'relatorio' && (
          <div className="mt-4">
            <h2 className="text-2xl font-semibold text-gray-200 mb-4">
              Relatório de Entradas ({filteredMaterials.length} de {materials.length})
            </h2>

            {/* Filtros */}
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
              <div>
                <label htmlFor="filterCarrier" className="block text-sm font-medium text-gray-300 mb-1">Filtrar Transportadora</label>
                <select
                  id="filterCarrier"
                  value={filterCarrier}
                  onChange={(e) => setFilterCarrier(e.target.value)}
                  className="w-full p-3 border border-gray-600 bg-gray-700 text-gray-200 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                >
                  {filterCarriersOptions.map((option, index) => (
                    <option key={index} value={option}>
                      {option}
                    </option>
                  ))}
                </select>
              </div>
              <div>
                <label htmlFor="filterShipment" className="block text-sm font-medium text-gray-300 mb-1">Filtrar Remessa</label>
                <input
                  type="text"
                  id="filterShipment"
                  value={filterShipment}
                  onChange={(e) => setFilterShipment(e.target.value)}
                  placeholder="Pesquisar remessa..."
                  className="w-full p-3 border border-gray-600 bg-gray-700 text-gray-200 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
              </div>
              <div>
                <label htmlFor="filterDate" className="block text-sm font-medium text-gray-300 mb-1">Filtrar por Data</label>
                <input
                  type="date"
                  id="filterDate"
                  value={filterDate}
                  onChange={(e) => {
                    const [year, month, day] = e.target.value.split('-');
                    setFilterDate(${day}/${month}/${year});
                  }}
                  className="w-full p-3 border border-gray-600 bg-gray-700 text-gray-200 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
              </div>
            </div>

            {filteredMaterials.length > 0 ? (
              <div className="max-h-96 overflow-y-auto border border-gray-700 rounded-md p-2">
                <table className="min-w-full divide-y divide-gray-700">
                  <thead className="bg-gray-700">
                    <tr>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">
                        Transportadora
                      </th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">
                        Remessa
                      </th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">
                        Data e Hora
                      </th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">
                        Status
                      </th>
                      <th className="px-6 py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">
                        Ações
                      </th>
                    </tr>
                  </thead>
                  <tbody className="bg-gray-800 divide-y divide-gray-700">
                    {filteredMaterials.map((material, index) => (
                      <tr key={index}>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-200">
                          {material.carrier}
                        </td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-50">
                          {material.shipment}
                        </td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-400">
                          {material.timestamp}
                        </td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-200">
                          <select
                            value={material.status}
                            onChange={(e) => handleStatusChange(index, e.target.value)}
                            className="w-full p-1 border border-gray-600 bg-gray-700 rounded-md focus:outline-none focus:ring-1 focus:ring-blue-500"
                          >
                            {statusOptions.map((option, i) => (
                              <option key={i} value={option}>
                                {option}
                              </option>
                            ))}
                          </select>
                        </td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-200 flex gap-2">
                          {material.status === 'RECUSADO(AVARIA)' && (
                            <button
                              onClick={() => handleGenerateAction(material)}
                              className="bg-purple-600 text-white px-3 py-1 rounded-md text-sm hover:bg-purple-500 transition duration-300 ease-in-out shadow-lg"
                            >
                              ✨ Ação
                            </button>
                          )}
                          <button
                            onClick={() => handleGenerateEmail(material)}
                            className="bg-blue-600 text-white px-3 py-1 rounded-md text-sm hover:bg-blue-500 transition duration-300 ease-in-out shadow-lg"
                          >
                            ✨ Email
                          </button>
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            ) : (
              <p className="text-center text-gray-400 mt-8">
                Nenhuma entrada encontrada com os filtros aplicados.
              </p>
            )}
          </div>
        )}

        {/* Modal de Duplicidade */}
        <DuplicateModal
          isOpen={showDuplicateModal}
          onClose={handleCloseDuplicateModal}
          shipment={duplicateShipment}
        />

        {/* Modal de Geração de Texto */}
        <TextModal
          isOpen={showTextModal}
          onClose={() => setShowTextModal(false)}
          title={textModalTitle}
          content={textModalContent}
        />
      </div>
    </div>
  );
};

export default App;